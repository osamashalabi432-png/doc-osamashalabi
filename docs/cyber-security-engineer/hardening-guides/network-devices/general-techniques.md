# Common Hardening Techniques

## General Techniques

Hardening techniques reduce the attack surface of a system or network by removing unnecessary functionality, limiting access, and implementing security controls. Standard methods:

- **Updating & Patching** — keeping the OS and applications on all devices current, with regular security patches applied. Outdated software contains known vulnerabilities that attackers actively exploit.
- **Disabling unnecessary services & ports** — turning off services and blocking ports (physical and virtual) not needed for the device's function. Fewer open entry points means fewer things to exploit.
- **Principle of Least Privilege (POLP)** — restricting users and processes to only the minimum permissions required to perform their functions.
- **Logs Monitoring** — implementing a log monitoring system to catch unusual activity or security events.
- **Regular backups** — routine backups of systems and configurations, so a security incident or system failure doesn't mean permanent loss.
- **Enforcing strong passwords** — changing default login passwords and using passwords at least ten characters long, combining lowercase, uppercase, special characters, and numbers, to protect against dictionary and brute-force attacks.
- **Multi-Factor Authentication (MFA)** — requiring two or more types of identification before granting account/system access — typically something you know (a password) and something you have (e.g. a biometric or hardware token).

## Importance of Secure Protocols

Secure protocols protect against unauthorized access and data breaches by ensuring sensitive data transmitted between devices is encrypted and can't be intercepted. They also help prevent man-in-the-middle attacks and other network-based exploits, so only authorized personnel can access sensitive information or perform administration tasks. Key secure protocols: **HTTPS, SSH, SSL/TLS, and IPsec**.

## Removal/Blocking of Insecure Protocols

Alongside using secure protocols, actively removing and blocking insecure ones reduces the attacker's available attack surface. Most important to remove are protocols that transmit data in clear text without encryption — **FTP, HTTP, Telnet, SMTP**, and similar. Note that some protocols considered "inherently secure" (e.g. LDAP, RDP, SIPS) can still be exploited by attackers if configured incorrectly, so secure-by-design doesn't mean secure-by-default.

## Implementation of Monitoring and Logging Controls

Logging on network devices is essential for detecting and investigating security incidents, identifying performance issues, and meeting regulatory requirements. It provides a record of events and activity that supports troubleshooting, forensic analysis, and auditing. Common logging techniques:

- **Syslog** — a protocol that standardizes the transfer of log messages, typically to a central server for storage and analysis.
- **SNMP** — sends a trap (notification) from a network device to a management system when a predefined event occurs.
- **NetFlow** — collects and analyzes network traffic data for monitoring and security analysis.
- **Packet Captures** — captures and stores network traffic for analysis with a tool like Wireshark.
