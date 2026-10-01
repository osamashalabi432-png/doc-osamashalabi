## 2. Review archived application versions
 
- [ ] Look at old snapshots of the site
Website: [Wayback Machine](https://web.archive.org) — enter the domain and browse snapshots by date.
 
List all archived snapshots:
 
```bash
curl -s "http://web.archive.org/cdx/search/cdx?url=onetec.com*&output=text&fl=original&collapse=urlkey" | sort -u
```
 
 
## 3. Identify historical parameters
 
- [ ] Find URL parameters used before (potential injection points)
```bash
cat urls.txt | grep "=" | sort -u > params.txt
```
 
Dedicated tool:
 
```bash
paramspider -d onetec.com
```
 
---
 
## 4. Identify deprecated endpoints
 
- [ ] Hunt for old paths that may still work
```bash
cat urls.txt | grep -E "old|legacy|deprecated|v1|test" | sort -u
```
 
Check which still respond:
 
```bash
cat urls.txt | httpx -sc -title
```
 
---
 
## 5. Search for old API endpoints
 
- [ ] Find API routes from the archives
```bash
cat urls.txt | grep -E "api|graphql|rest|/v[0-9]" | sort -u
```
 
---
 
## 6. Look for exposed backup files
 
- [ ] Search for backup and config files left on the server
Filter the archive list:
 
```bash
cat urls.txt | grep -E "\.bak|\.zip|\.tar|\.sql|\.old|\.backup|\.env|\.config" | sort -u
```
 
Brute-force common ones:
 
```bash
feroxbuster -u https://onetec.com -w /usr/share/wordlists/dirb/common.txt -x bak,zip,sql,old,tar,gz
```
 
---
 
## 7. Review historical JavaScript files
 
- [ ] Old JS files often leak API keys, endpoints, and secrets
Pull JS files from the archive:
 
```bash
cat urls.txt | grep "\.js" | sort -u > jsfiles.txt
```
 
Scan them for exposures:
 
```bash
cat jsfiles.txt | httpx -sc | nuclei -t exposures/
```
 
Extract links from each JS file:
 
```bash
cat jsfiles.txt | while read url; do echo $url; curl -s $url | grep -Eo "(http|https)://[a-zA-Z0-9./?=_-]*"; done
```
 
---
 
## Quick flow (copy-paste order)
 
```bash
gau onetec.com > urls.txt
cat urls.txt | grep "=" | sort -u > params.txt
cat urls.txt | grep "\.js" | sort -u > jsfiles.txt
cat urls.txt | grep -E "\.bak|\.sql|\.zip|\.env" | sort -u > backups.txt
```
