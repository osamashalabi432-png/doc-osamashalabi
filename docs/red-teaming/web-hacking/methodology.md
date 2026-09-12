# Web Hacking — Methodology

!!! warning "Authorized testing only"
    This methodology and everything under Web Hacking is personal notes from authorized labs, CTFs (HTB/THM), and bug-bounty programs. Only use these steps against systems you own or have explicit written permission to test.

A penetration test is not "just start hacking" — it's a repeatable sequence of phases, each one building on what the previous phase learned. Skipping a phase (e.g. going straight to exploitation without enumeration) usually means missing the easiest way in.

## The five phases

1. **Reconnaissance** — passive and active information gathering about the target (see [Reconnaissance](reconnaissance.md) and [Information Gathering](information-gathering.md)).
2. **Scanning and enumeration** — identify live hosts, open ports, and running services in detail, using tools like Nmap, Nessus, and Nikto (see [Scanning & Enumeration](scanning-enumeration.md)).
3. **Gain Access** — exploit a discovered weakness to get a foothold (see the vulnerability-class pages, e.g. [File Upload](file-upload.md)).
4. **Maintaining access** — keep a way back into the system after the initial foothold.
5. **Covering tracks** — clean up evidence of the test activity.

!!! note "Why this order matters"
    Each phase feeds the next: recon tells you *what* exists, scanning/enumeration tells you *what's running and how it's configured*, and only then does exploitation have enough information to target the right weakness. Jumping straight to "Gain Access" without the first two phases means guessing blind.
