
## 1. Map applications and sub-applications

- [ ] Find all apps and separate areas under the domain

```bash
katana -u https://onetec.com -d 5 -o crawl.txt
```

Also check subdomains from your earlier recon — each may host a separate app.

---

## 2. Map authentication boundaries

- [ ] Find where login is required vs. public

```bash
cat crawl.txt | pdhttpx -sc -title
```

---

## 3. Identify user roles

- [ ] Work out the different permission levels (user, admin, guest)

Look for role hints in the app: signup options, account types, and paths like `/admin`, `/dashboard`, `/user`.

```bash
cat crawl.txt | grep -iE "admin|user|account|dashboard|role|manage"
```

---

## 4. Identify administrative functionality

- [ ] Find admin panels and privileged features

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -mc 200,301,302
```

Common guesses: `/admin`, `/administrator`, `/wp-admin`, `/manage`, `/console`, `/panel`.

---

## 5. Identify API endpoints

- [ ] Map all API routes

```bash
cat crawl.txt | grep -iE "api|graphql|/v[0-9]|\.json" | sort -u
```

Pull endpoints from JS files too:

```bash
cat jsfiles.txt | while read u; do curl -s $u | grep -Eo "/[a-zA-Z0-9_/-]*api[a-zA-Z0-9_/-]*"; done | sort -u
```

---

## 6. Identify upload functionality

- [ ] Find file upload points (common vuln spot)

```bash
cat crawl.txt | grep -iE "upload|file|attach|import|media"
```

Also search page forms for `type="file"`.

---

## 7. Identify download functionality

- [ ] Find download features (path traversal / LFI risk)

```bash
cat crawl.txt | grep -iE "download|export|file=|path=|getfile"
```

---

## 8. Identify import functionality

- [ ] Find import features (CSV, XML, etc.)

```bash
cat crawl.txt | grep -iE "import|upload|csv|xml|batch"
```

---

## 9. Identify export functionality

- [ ] Find export features (data leak / injection spots)

```bash
cat crawl.txt | grep -iE "export|report|generate|download|csv|pdf"
```

---

## 10. Identify search functionality

- [ ] Find search boxes and search endpoints

```bash
cat crawl.txt | grep -iE "search|query|q=|find|lookup"
```

---

## 11. Identify integrations

- [ ] Find connections to third-party services

```bash
cat crawl.txt | grep -iE "oauth|connect|integration|callback|sso"
```

---

## 12. Identify webhooks

- [ ] Find webhook endpoints (SSRF risk)

```bash
cat crawl.txt | grep -iE "webhook|callback|notify|hook"
```

---

## 13. Identify redirects

- [ ] Find redirect parameters (open redirect risk)

```bash
cat urls.txt | grep -iE "redirect|url=|next=|return=|dest=|continue=|goto="
```

---

## 14. Identify URL processing functionality

- [ ] Find features that fetch or process URLs (SSRF risk)

```bash
cat urls.txt | grep -iE "url=|uri=|link=|fetch|proxy|load=|src="
```

---

## Quick flow (copy-paste order)

```bash
katana -u https://onetec.com -d 5 -o crawl.txt
cat crawl.txt | httpx -sc -title -o live_endpoints.txt
cat crawl.txt | grep -iE "api|graphql|/v[0-9]" | sort -u > apis.txt
cat crawl.txt | grep -iE "upload|download|import|export" | sort -u > filefuncs.txt
cat urls.txt | grep -iE "redirect|url=|next=|dest=" | sort -u > redirects.txt
```