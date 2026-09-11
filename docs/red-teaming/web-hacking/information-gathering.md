# Web Hacking — Information Gathering

!!! warning "Authorized testing only"
    These are personal notes from authorized labs, CTFs, and bug-bounty programs. Only run these lookups against targets you own or have explicit written permission to test.

Information gathering is about building a picture of a target's people, infrastructure, and exposed data — mostly using free/public tools and services, so it can usually be done passively. This page collects the practical tools/sites used for email discovery, subdomain enumeration, technology fingerprinting, and breach-data lookups. For the overall recon workflow (target validation, DNS, active host/port discovery), see [Reconnaissance](reconnaissance.md).

## Email addresses

- **Why start here:** valid email addresses are useful both for social-engineering/phishing scope and as usernames to try against login forms later.
- [hunter.io](http://hunter.io) — get email addresses associated with a specific domain/organization (e.g. a university).
- [phonebook.cz](http://phonebook.cz) — get all discoverable emails for a specific domain.
- [dehashed.com](http://dehashed.com) — search for already-breached/hashed passwords tied to a target.
- To gather email addresses automatically from public sources:

```bash
theHarvester -d <Domain.com>
```

## Subdomains

- **Why this matters:** subdomains often run older/forgotten apps (staging, admin panels, dev environments) that get less attention than the main site — they expand the attack surface significantly.
- **sublist3r** — enumerates subdomains for a domain:

```bash
sublist3r -d <Domain.com> -e
```

- **crt.sh** — searches Certificate Transparency logs for subdomains that ever had a TLS certificate issued for them.
- **assetfinder** — built into Kali, also useful for finding subdomains.

## Website technologies (fingerprinting)

- **Why fingerprint the stack first:** knowing the CMS/framework/server software in use lets later vulnerability research be targeted (e.g. known CVEs for that exact version) instead of guessing generically.
- [builtwith.com](http://builtwith.com) — identifies the technology stack behind a website.
- **Wappalyzer** — browser extension that does the same, live, while browsing.
- **whatweb** — built into Kali:

```bash
whatweb <Domain.com>
```

## Data breaches

- **Why check breach data:** if the target's employees/users appear in past breaches, their reused credentials may work against the current target — this is pure OSINT, no direct interaction with the target needed.
- [haveibeenpwned.com](https://haveibeenpwned.com/) — check if an email/domain shows up in known breaches:

```bash
https://haveibeenpwned.com/
```
