# Network Management

## Windows Defender Firewall

Windows Defender Firewall is a built-in application that protects computers from malicious attacks and blocks unauthorized traffic through inbound and outbound rules or filters — conceptually equivalent to controlling "who is coming in and going out of your home."

Malicious actors abuse Windows Firewall by bypassing existing rules. For example, if the firewall is configured to allow incoming connections, attackers will try to manipulate that functionality to create a remote connection to the victim's computer.

Access Windows Defender Firewall via `WF.msc` in the Run dialog.

## Disable Unused Networking Devices

Every enabled network adapter is a potential entry point. Disable ones that aren't in use via `Control Panel > System and Security Setting > System > Device Manager`, then disable all unused networking devices there.

## Disable SMB1 Protocol

SMB is a file-sharing protocol that has been repeatedly exploited by attackers in the wild (it was the vector for major worms such as WannaCry). Since it's primarily used for file sharing within a network, you should disable it if the computer isn't part of a network that requires it.

!!! important "Why this matters"
    SMBv1 is an old, insecure version of the protocol with known critical vulnerabilities. Disabling it removes an entire class of remote exploitation risk without affecting SMBv2/v3 file sharing.

From an Administrator PowerShell prompt:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

## Protecting Local Domain Name System (DNS)

DNS translates Fully Qualified Domain Names (FQDNs) into IP addresses. The local hosts file — located at `C:\Windows\System32\Drivers\etc\hosts` — lets you statically map hostnames to IPs, bypassing DNS resolution for specific entries and protecting against DNS-based redirection for those names.

## Mitigating Address Resolution Protocol (ARP) Attacks

ARP resolves MAC addresses from IP addresses and caches the results in the workstation's ARP cache. Because ARP has no built-in authentication, it's a common target for spoofing attacks that redirect traffic through an attacker's machine.

Check current ARP entries with:

```text
arp -a
```

Example output:

```text
Interface: 192.168.231.2 --- 0x5
  Internet Address      Physical Address      Type
  192.168.231.255       ff-ff-ff-ff-ff-ff     static
  224.0.0.2             01-00-5e-00-00-02     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
```

## Preventing Remote Access to the Machine

Remote access provides a way to connect to other computers/networks — even at a different geographical location — for file sharing and remote changes. If it isn't required, disable it via `Settings > Remote Desktop`.

!!! tip
    Disabling Remote Desktop when it's not needed removes a commonly targeted service (RDP is one of the most brute-forced and exploited remote-access protocols on the internet).
