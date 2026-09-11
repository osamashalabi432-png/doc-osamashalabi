# Active Directory Hardening

Baseline hardening for Active Directory environments: understanding the core AD structures, securing the authentication protocols that AD relies on, implementing least privilege through account types and tiered access, applying Microsoft's official security baselines, and closing off the attack paths most commonly used against AD.

| Topic | Description |
|---|---|
| [General Concepts](general-concepts.md) | Domain, Domain Controller, Trees, and Forests — the structures everything else builds on. |
| [Securing Authentication Methods](authentication.md) | LAN Manager hash, SMB signing, LDAP signing, password rotation, and password policy settings. |
| [Implementing the Least Privilege Model](least-privilege-tiering.md) | Account types and the Tiered Access Model (Tier 0/1/2). |
| [Microsoft Security Compliance Toolkit](security-baselines.md) | Installing official security baselines and using the Policy Analyser. |
| [Protecting Against Known Attacks](known-attacks.md) | Kerberoasting, weak passwords, RDP brute-forcing, and publicly accessible shares. |
