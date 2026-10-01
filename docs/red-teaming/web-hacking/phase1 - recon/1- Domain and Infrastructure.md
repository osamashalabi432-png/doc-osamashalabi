## 1. Identify the primary domain
 
- [ ] Confirm the root domain and who owns it

- Confirm the root domain (strip any subdomain, e.g. `app.onetec.com` → `onetec.com`). 
- Watch for two-part endings like `.co.uk`.
 
```bash
whois onetec.com
```

```bash
dig +short onetec.com
```
---
## 2. Enumerate subdomains
 
- [ ] Find all subdomains under the root
```bash
subfinder -d onetec.com -o subs.txt
```
 
Alternatives for more coverage:
 
```bash
amass enum -d onetec.com
assetfinder --subs-only onetec.com
```
 ---
 ## 3. Resolve discovered domains
 
- [ ] Check which subdomains point to a real IP
```bash
dnsx -l subs.txt -o resolved.txt
```
 
