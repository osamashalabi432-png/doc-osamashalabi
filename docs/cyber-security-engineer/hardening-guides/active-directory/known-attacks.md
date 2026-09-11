# Protecting Against Known Attacks

## Kerberoasting

[Kerberoasting](https://tryhackme.com/room/attackingkerberos) is a common and successful post-exploitation technique attackers use to gain privileged access to AD. The attacker abuses the Kerberos Ticket Granting Service (TGS) to request a service ticket, which is encrypted with the target service account's password hash, then cracks that hash offline via brute force.

These attacks are difficult to detect because the request is made through an already-approved, authenticated user, and no unusual traffic pattern is generated in the process.

!!! important "Mitigation"
    Ensure an additional layer of authentication through MFA, and reset Kerberos Key Distribution Center (KDC) / service account passwords frequently and periodically. Long, complex service account passwords also make offline cracking impractical even if a ticket is captured.

## Weak and Easy-to-Guess Passwords

Weak, reused, or previously-breached passwords are the easiest target for intruders. A strong password combines uppercase and lowercase letters, numbers, and special characters — and should never overlap with known-compromised passwords. Password auditing tools exist specifically to identify accounts in AD using weak or compromised passwords, so they can be flagged and reset before an attacker finds them.

## Brute-Forcing Remote Desktop Protocol (RDP)

Attackers use scanning tools to brute force weak RDP credentials. Once successful, they quickly access the compromised system and attempt privilege escalation and persistence.

!!! important "Mitigation"
    Never expose RDP directly to the public internet without additional security controls (VPN, MFA, jump host). Continuously audit for scanning and brute-force attempts against exposed services.

## Publicly Accessible Shares

During AD configuration, some share folders end up publicly accessible or left unauthenticated — giving attackers an initial foothold for lateral movement once they've compromised any account on the network.

Use the `Get-SmbOpenFile` PowerShell cmdlet to look for undesired shares on the network and configure access accordingly:

```powershell
Get-SmbOpenFile
```
