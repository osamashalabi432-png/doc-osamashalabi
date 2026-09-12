# Web Hacking — Reconnaissance

Reconnaissance is the information-gathering phase before any active testing — the goal is to build a picture of the target (who owns it, what it runs, where it's exposed) without necessarily touching it yet. It's split into **passive** recon (no direct interaction that the target could notice) and **active** recon (direct interaction, like port scans, that the target's logs/IDS could see).

For the specific tools used to enumerate subdomains, fingerprint technology, and check breach data, see [Information Gathering](information-gathering.md). This page covers the overall recon workflow, target validation, DNS/host discovery, and Google dorking.

## Passive recon

- When targeting a physical location, passive recon looks for two categories of information: **location information** (satellite images, drone recon, building layout) and **job information** (employee names/job titles/phone numbers/managers, or badge/desktop/computer photos).
- When targeting a web app/host, passive recon covers four areas:
    1. **Target validation** — confirm ownership/scope: `WHOIS`, `nslookup`, `dnsrecon`.
    2. **Finding subdomains** — Google fu, Nmap, sublist3r, bluto, crt.sh (see [Information Gathering](information-gathering.md)).
    3. **Fingerprinting** — Nmap, Wappalyzer, WhatWeb, BuiltWith, netcat (see [Information Gathering](information-gathering.md)).
    4. **Data breaches** — HaveIBeenPwned, BreachParse, WeLeakInfo (see [Information Gathering](information-gathering.md)).

### Target validation and DNS lookups

- **Why check this first:** before anything else, confirm the IP/domain actually belongs to the target and pull basic ownership/registration data — testing the wrong asset is a scope violation.

```bash
whois <Domain.com>
```

- To resolve a domain's IP address:

```bash
host <Domain.com>
```

- DNS recon — pulls DNS records (A, MX, NS, TXT, etc.) to map out the target's infrastructure:

```bash
dnsrecon -d <Domain.com>
```

- A more thorough/visual DNS recon option:

```text
dnsdumpster.com
```

- To gather more information about a website/host (netblock, hosting provider, etc.):

```text
netcraft.com
```

### Checking for exposed files and WAFs

- `robots.txt` and `sitemap.xml`/`sitemap_index.xml` are often overlooked — `robots.txt` tells crawlers (and us) which paths the owner doesn't want indexed, which can hint at admin/sensitive paths; `sitemap_index.xml` helps search engines (and us) index the whole site structure at once.

```text
robots.txt
```

```text
sitemap_index.xml
```

- **Why check for a WAF before active scanning:** knowing a Web Application Firewall is in front of the target changes how (and how carefully) later active scanning/exploitation should be done, to avoid getting blocked or triggering alerts.

```bash
wafw00f <Domain.com>
```

- To download/mirror an entire website for offline review:

```text
HTTrack
```

## Google dorking

Google dorking uses search-engine operators to surface pages/files that are indexed but not obviously linked from the site's navigation — a fast, completely passive way to find exposed content.

- Restrict search results to a specific site:

```text
site:<Domain.com>
```

- Search all subdomains of a target:

```text
site:<*.Domain.com>
```

- Search for a specific admin path indexed under the domain:

```text
site:<Domain.com>:admin
```

- Search for forum content indexed under the domain:

```text
site:<Domain.com>:forum
```

- Search for a specific file type (e.g. leaked CSV exports):

```text
type: csv
```

## Active recon

Active recon means directly interacting with the target's infrastructure — this is noisier (can show up in logs/IDS) but gives concrete, current data instead of indexed/cached information.

### DNS enumeration

```bash
dnsenum <Domain.com>
```

### Host discovery

- **Why do host discovery before port scanning:** on a network range, scanning every possible host for open ports wastes time — finding which hosts are actually alive first narrows the target list.

```bash
sudo nmap -sn <IP_TARGET>
```

- Alternative host-discovery method:

```bash
sudo netdiscover -i eth0 -r <IP_TARGET>
```

- Host discovery across an entire subnet:

```bash
nmap -sn <NETWORK-IP>
```

### Port scanning

- Basic port scan against a single port:

```bash
nmap -p 8080 <IP-target>
```

- **How to tell if a firewall is present:** if a scanned port comes back **closed** (not filtered), that's a sign there's no firewall blocking it — a firewall typically shows as *filtered* instead of *closed*.

### Nmap options reference

| Flag | Purpose |
|---|---|
| `-O` | Guess the OS of the target |
| `-sS` | Stealth (SYN) scanning |
| `--osscan-guess` | Guess the kernel version |
| `--version-intensity 8` | Aggressiveness of service version detection |
| `-sA` | Determine whether a firewall is stateful and which ports it's blocking |
| `-f` | Attempt to evade naive IDS/IPS or firewalls that don't reassemble fragments for inspection |
| `--mtu n` | Controls the fragment size |
| `-F` | Scans a reduced list of ports |
| `-v` | Increase verbosity of the result |
| `-oN` | Normal, human-readable output |
| `-oX` | XML output (machine-readable; useful for tools & parsing) |
| `-oG` | Grepable output |
| `-oA` | Create all three output formats at once |
| `-oJ` | JSON output |

For the deeper, port-by-port enumeration workflow (Metasploit auxiliary modules, SMB/FTP/SSH/SMTP/MySQL, Nmap-driven walkthroughs), see [Scanning & Enumeration](scanning-enumeration.md).
