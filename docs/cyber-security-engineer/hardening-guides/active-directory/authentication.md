# Securing Authentication Methods

## LAN Manager Hash

Windows account passwords are never stored in clear text — instead, Windows stores password hashes. When a password shorter than 15 characters is changed or set, Windows generates and can store **both** an LM (LAN Manager) hash and an NT (Windows NT) hash.

!!! important "Why this matters"
    The LM hash is significantly weaker than the NT hash and is vulnerable to fast brute-force attacks (it splits passwords into two 7-character halves and uses a weak character set, both of which make it far cheaper to crack). If both hashes exist, an attacker who obtains them will crack the weaker LM hash first.

Prevent Windows from storing the LM hash via:

```text
Group Policy Management Editor > Computer Configuration > Policies > Windows Settings >
Security Settings > Local Policies > Security Options >
"Network security: Do not store LAN Manager hash value on next password change" > Define policy setting
```

## SMB Signing

SMB (Server Message Block) is the protocol Microsoft-based networks primarily use for file and print communication. SMB signing cryptographically signs SMB traffic so both client and server can verify its integrity — this is what detects Man-in-the-Middle (MiTM) attacks that attempt to modify SMB traffic in transit. All supported Windows versions include an SMB packet-signing option.

Enable it via:

```text
Group Policy Management Editor > Computer Configuration > Policies > Windows Settings >
Security Settings > Local Policies > Security Options >
"Microsoft network server: Digitally sign communications (always)" > Enable
```

## LDAP Signing

LDAP (Lightweight Directory Access Protocol) is used to locate and authenticate resources on the network. Because LDAP requests can otherwise be replayed or manipulated via MiTM attacks, LDAP signing — a Simple Authentication and Security Layer (SASL) property — ensures only signed LDAP requests are accepted, and plain-text/non-SSL requests are ignored.

Enable it via:

```text
Group Policy Management Editor > Computer Configuration > Policies > Windows Settings >
Security Settings > Local Policies > Security Options >
"Domain controller: LDAP server signing requirements" > Require signing
```

## Password Rotation

Rotating passwords regularly is important, but manually resetting and propagating passwords across an organization is difficult enough that many organizations skip it. Three approaches, each with different trade-offs:

- **Scripted rotation** — a PowerShell script run on a Scheduled Task automatically updates passwords. Requires no additional infrastructure, but the script itself has to be written and maintained, which can become its own liability.
- **Multi-Factor Authentication (MFA)** — adding an MFA layer reduces reliance on frequent password rotation as the primary defense, since a leaked password alone is no longer sufficient to authenticate.
- **Group Managed Service Accounts (gMSAs)** — a Microsoft-provided mechanism specifically for service account passwords; gMSA passwords rotate automatically every 30 days without manual intervention.

## Password Policies

Attackers use a range of password-compromise techniques against corporate accounts — brute force, dictionary attacks, password spraying, and credential stuffing. A strict, organization-wide password policy (length, complexity, change frequency) is the baseline defense against all of them.

Configure it via:

```text
Group Policy Management Editor > Computer Configuration > Policies > Windows Settings >
Security Settings > Account Policies > Password Policy
```

### Recommended Settings

| Setting | Recommendation |
|---|---|
| Enforce password history | Prevent at least 10–15 old passwords from being reused |
| Minimum password length | 10–14 characters |
| Complexity requirements | Must not contain the account's username; must include uppercase, lowercase, digits, and special characters |
