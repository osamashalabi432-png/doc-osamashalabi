## 1. Identify web server

- [ ] Find the server software (Apache, Nginx, IIS)

```bash
whatweb https://onetec.com
```

---

## 2. Identify application framework

- [ ] Detect the backend framework (Laravel, Django, Rails, Express)

```bash
whatweb -a 3 https://onetec.com
```

Clues also show in headers like `X-Powered-By` and in cookie names:

```bash
curl -sI https://onetec.com | grep -iE "x-powered-by|set-cookie"
```

---

## 3. Identify programming language

- [ ] Work out the language (PHP, Python, Node, .NET)

```bash
wappalyzer https://onetec.com
```

Hints: file extensions (`.php`, `.aspx`), the `X-Powered-By` header, and session cookie names (`PHPSESSID`, `JSESSIONID`, `ASP.NET_SessionId`).

---

## 4. Identify CMS

- [ ] Detect a CMS (WordPress, Joomla, Drupal)

```bash
whatweb https://onetec.com
```

For WordPress specifically:

```bash
wpscan --url https://onetec.com --enumerate
```

Generic CMS detection:

```bash
cmseek -u https://onetec.com
```

---

## 5. Identify JavaScript frameworks

- [ ] Detect front-end frameworks (React, Vue, Angular)

```bash
wappalyzer https://onetec.com
```

Or inspect the page source for framework signatures:

```bash
curl -s https://onetec.com | grep -iE "react|vue|angular|next|nuxt"
```

---

## 6. Identify API technologies

- [ ] Spot REST, GraphQL, or SOAP endpoints

```bash
curl -s https://onetec.com | grep -iE "api|graphql|/v[0-9]"
```

Test for a GraphQL endpoint:

```bash
curl -s https://onetec.com/graphql -X POST -d '{"query":"{__typename}"}' -H "Content-Type: application/json"
```

---

## 7. Identify authentication technologies

- [ ] Detect login/auth tech (OAuth, JWT, SAML, SSO)

```bash
curl -sI https://onetec.com | grep -iE "www-authenticate|authorization"
```

Look for JWTs in cookies/storage, and for `/oauth`, `/saml`, `/login` paths in your URL list.

---

## 8. Identify third-party services

- [ ] Find external services (analytics, CDNs, payment, chat)

```bash
wappalyzer https://onetec.com
```

Or pull external domains from the page:

```bash
curl -s https://onetec.com | grep -Eo "https?://[a-zA-Z0-9./?=_-]*" | sort -u
```

---

## 9. Identify exposed software versions

- [ ] Find version numbers that leak in headers or pages

```bash
whatweb -a 3 https://onetec.com
```

```bash
nuclei -u https://onetec.com -t technologies/
```

---

## 10. Check for outdated components

- [ ] Flag old, vulnerable versions

```bash
nuclei -u https://onetec.com -t cves/ -t vulnerabilities/
```

For JS libraries specifically:

```bash
nuclei -u https://onetec.com -t technologies/ -t exposures/
```

---

## 11. Review HTTP response headers

- [ ] Read all headers for tech and security clues

```bash
curl -sI https://onetec.com
```

Full header + security header check:

```bash
nuclei -u https://onetec.com -t misconfiguration/http-missing-security-headers.yaml
```

---

## 12. Review error messages for technology disclosure

- [ ] Trigger errors that leak stack traces or versions

Request a page that likely 404s, and read the error body:

```bash
curl -s https://onetec.com/thispagedoesnotexist123
```

Send a malformed request to provoke a verbose error:

```bash
curl -s "https://onetec.com/?id[]=1"
```

> [!note]
> Look for framework names, file paths, SQL errors, or version strings in the response.

---

## Quick flow (copy-paste order)

```bash
curl -sI https://onetec.com
whatweb -a 3 https://onetec.com
wappalyzer https://onetec.com
nuclei -u https://onetec.com -t technologies/ -t cves/
```

> [!tip] Tool notes
> - `nuclei`, `httpx` → ProjectDiscovery Go tools (`go install ...`, needs Go 1.21+)
> - `whatweb`, `wpscan`, `cmseek` → usually preinstalled on Kali or `sudo apt install`
> - `wappalyzer` → CLI version via npm, or use the browser extension

> [!warning]
> Keep all activity inside your authorized scope.