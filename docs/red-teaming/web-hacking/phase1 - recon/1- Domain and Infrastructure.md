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
 
- [x] Find all subdomains under the root
```bash
subfinder -d onetec.com -o subs.txt
```
 
Alternatives for more coverage:
 
```bash
amass enum -d onetec.com
assetfinder --subs-only onetec.com
```
 ---
 ## Resolve discovered domains
 
- [x] Check which subdomains point to a real IP
```bash
dnsx -l subs.txt 
```
 ---
 
## 4. Identify live HTTP and HTTPS services
 
- [x] Find which resolved hosts serve a working site
```bash
httpx -l resolved.txt -sc -title -o live.txt
```
 
---
 
## 5. Identify exposed ports and services
 
- [x] Scan for open ports
```bash
nmap -sV onetec.com
```
 
Faster for many hosts:
 
```bash
naabu -l resolved.txt -o ports.txt
```
 
---
 
## 6. Identify IP addresses and hosting providers
 
- [x] Find the IPs and who hosts them
```bash
dig +short onetec.com
whois <IP-address>
asnmap -d onetec.com
```
 
---
 
## 7. Review DNS records
 
- [ ] Pull the main record types
```bash
dig +short A onetec.com
dig +short MX onetec.com
dig +short NS onetec.com
dig +short TXT onetec.com
```
 
---
 
## 8. Check certificate transparency logs
 
- [ ] Find subdomains and related domains from certificates
Website: search `onetec.com` on [crt.sh](https://crt.sh)
 
Command line:
 
```bash
curl -s "https://crt.sh/?q=onetec.com&output=json" | jq -r '.[].name_value' | sort -u
```
 
---
 
## 9. Identify CDN usage
 
- [ ] Check for a CDN layer (Cloudflare, Akamai, Fastly)
```bash
cdncheck -i <IP>
```
 
Manual check:
 
```bash
dig +short onetec.com
whois <the-IP>
```
 
---
 
## 10. Identify reverse proxies
 
- [ ] Look at response headers for proxy clues
```bash
curl -sI https://onetec.com
```
 
> [!note]
> Check headers like `Via`, `X-Cache`, and `Server`.
 
---
 
## 11. Identify WAF protection
 
- [ ] Detect a web application firewall
```bash
wafw00f https://onetec.com -a
```
 
---
 
## 12. Search for development and staging environments
 
- [ ] Hunt for `dev`, `staging`, `test`, `uat`, `qa` hosts
```bash
grep -E "dev|staging|test|uat|qa|preprod" subs.txt
```
