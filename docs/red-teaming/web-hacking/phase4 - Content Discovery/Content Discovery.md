## Wordlists worth knowing

Before fuzzing, know your wordlists. These ship with Kali or come from SecLists:

```bash
# Install SecLists if missing
sudo apt install seclists
```

- `/usr/share/wordlists/dirb/common.txt` — small, fast
- `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt` — directories
- `/usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt` — files
- `/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt` — big, thorough

---

## 1. Enumerate directories

- [ ] Brute-force directory names

With `ffuf`:

```bash

ffuf -u https://onetec.com/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -mc 200,204,301,302,307,401,403 -recursion -recursion-depth 5
```

With `feroxbuster` (recursive by default):

```bash
feroxbuster -u https://onetec.com -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -d 2
```

---

## 2. Enumerate files

- [ ] Brute-force filenames with extensions

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt -e .php,.html,.txt,.json,.bak,.zip -mc 200,301,302,403 -o files.txt
```

---

## 3. Search for hidden endpoints

- [ ] Find unlinked paths and routes

Combine crawling with fuzzing, and reuse archive URLs from Historical Discovery:

```bash
katana -u https://onetec.com -d 3 | pdhttpx -mc 200,403
cat urls.txt | pdhttpx -sc -title
```

---

## 4. Search for administrative interfaces

- [ ] Find admin panels

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/AdminPanels.txt -mc 200,301,302,401,403
```

Common guesses: `/admin`, `/administrator`, `/wp-admin`, `/manage`, `/console`, `/panel`, `/cpanel`.

---

## 5. Search for debug interfaces

- [ ] Find debug/dev endpoints

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200,403 -e .php
```

Targets: `/debug`, `/test`, `/phpinfo.php`, `/actuator`, `/__debug__`, `/trace`, `/status`.

---

## 6. Search for API documentation

- [ ] Find API docs, including Swagger/OpenAPI

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt -mc 200,403
```

Common doc paths to check directly:

```
/swagger-ui.html   /swagger.json   /openapi.json
/api-docs          /api/docs       /redoc        /v2/api-docs
```

---

## 7. Search for GraphQL endpoints

- [ ] Find and probe GraphQL

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/graphql.txt -mc 200,400,403
```

Test a found endpoint:

```bash
curl -s https://onetec.com/graphql -X POST -H "Content-Type: application/json" -d '{"query":"{__schema{types{name}}}"}'
```

> [!tip]
> If introspection works, you just mapped the whole API.

---

## 8. Search for source maps

- [ ] Find `.map` files (they rebuild original source code)

```bash
cat jsfiles.txt | sed 's/$/.map/' | pdhttpx -sc -mc 200
```

> [!note]
> A reachable `.js.map` can expose original front-end source, comments, and sometimes secrets.

---

## 9. Search for configuration files

- [ ] Find exposed config files

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt -e .env,.config,.conf,.ini,.yaml,.yml,.xml -mc 200
```

High-value: `.env`, `config.php`, `web.config`, `wp-config.php`, `application.yml`, `settings.py`.

---

## 10. Search for backup files

- [ ] Find backups left on the server

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt -e .bak,.old,.backup,.zip,.tar,.tar.gz,.sql,.save,~ -mc 200
```

Also try backups of known files (e.g. `index.php.bak`, `config.php~`).

---

## 11. Search for temporary files

- [ ] Find temp/swap files

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -e .tmp,.temp,.swp,.swo,.part -mc 200
```

> [!note]
> Editor swap files like `.index.php.swp` can leak full source.

---

## 12. Search for log files

- [ ] Find exposed logs

```bash
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -e .log -mc 200
```

Check directly: `/error.log`, `/access.log`, `/debug.log`, `/logs/`.

---

## 13. Review .well-known resources

- [ ] Check standardized metadata paths

```bash
curl -s https://onetec.com/.well-known/security.txt
curl -s https://onetec.com/.well-known/openid-configuration
```

---

## Quick flow (copy-paste order)

```bash
curl -s https://onetec.com/robots.txt
curl -s https://onetec.com/sitemap.xml
feroxbuster -u https://onetec.com -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -d 2
ffuf -u https://onetec.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt -e .env,.bak,.zip,.sql,.log,.config -mc 200,403 -o sensitive.txt
```



