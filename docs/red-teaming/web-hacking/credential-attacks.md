# Web Hacking — Credential Attacks

!!! warning "Authorized testing only"
    These are personal notes from authorized labs, CTFs, and bug-bounty programs. Only run these techniques against accounts/systems you own or have explicit written permission to test.

Credential attacks try to get valid login credentials without exploiting a code-level vulnerability — either by guessing passwords directly, reusing credentials leaked elsewhere, or cracking a hash that was already obtained. See [Scanning & Enumeration](scanning-enumeration.md) for the per-service Metasploit brute-force modules (SMB, FTP, SSH, SMTP, MySQL).

## Brute force (Hydra)

- **What this is:** systematically trying many username/password combinations against a login service until one works.

```bash
hydra -l root -P /usr/share/wordlist/metasploit/passwords.txt ssh://192.168.1.134:22 -t 4 -V
```

- `-l` → the username to try.
- `-P` → the password wordlist.
- `-t` → number of parallel threads.

## Credential stuffing

- Injecting credentials that were **breached from a different service** in hopes of account takeover — this relies on password reuse across sites, not on guessing new passwords. See [Information Gathering → Data breaches](information-gathering.md#data-breaches) for where breached credentials can be sourced.

## Hash identification

- Before a hash can be cracked, its type needs to be known. **hash-identifier** (built into Kali Linux) identifies the hash format from the hash string itself.
- See the [Scanning & Enumeration → Nmap practical example (Academy)](scanning-enumeration.md#nmap-practical-example-academy-thm) for a real walkthrough that used hash-identifier to crack a leaked hash into working credentials.
