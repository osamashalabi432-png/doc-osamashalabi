# doc-osamashalabi

Personal cybersecurity documentation site — deployments, implementation notes,
commands, troubleshooting, labs, and red teaming research. Built with
[MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

Live at: **doc.osamashalabi.com** (once deployed)

## Structure

```
docs/
  index.md                       Home page (two entry cards)
  cyber-security-engineer/       Enterprise security products, by category
    siem/fortisiem/
    firewall/fortigate/
    waf/fortiweb/
    edr/fortiedr/
    security-management/{fortimanager,fortianalyzer}/
    patch-management/ivanti/
    pam/{one-identity,delinea}/
    cloud/{azure,aws}/
  red-teaming/                   Offensive security research
    ai-red-teaming/
    web-hacking/
    os-hacking/{linux,windows}/
  stylesheets/extra.css          Minimal theme overrides
mkdocs.yml                       Site config, theme, nav, plugins
requirements.txt                 Pinned Python deps
```

Every product/technology page is currently a **placeholder** — no real
architecture details, commands, ports, or licensing info has been invented.
Content gets filled in per page from verified sources as it's written.

### Adding a new product or technology

The information architecture is designed not to need a redesign:

1. Pick (or create) the right **project category** folder under
   `cyber-security-engineer/` (e.g. `siem/`, `edr/`) — categories are by
   function, never by vendor.
2. Add a new folder for the product under that category
   (e.g. `siem/splunk/`), with an `index.md` (Overview) plus whatever pages
   make sense for that product — you don't have to match another product's
   page list exactly.
3. Add the corresponding entries to `nav:` in `mkdocs.yml`, following the
   existing indentation pattern.
4. For Red Teaming, the same applies under `red-teaming/<topic>/`.

## Local development

Requires Python 3.10+.

```bash
python -m venv .venv
.venv/Scripts/activate     # Windows
source .venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
mkdocs serve
```

Then open http://127.0.0.1:8000.

> **Note (this machine):** the system had no working Python installer, so a
> standalone embeddable Python 3.12 was set up at
> `%LOCALAPPDATA%\Programs\PythonEmbed312` with pip and mkdocs-material
> installed directly into it (no venv support in the embeddable
> distribution). To serve locally here, run:
> `%LOCALAPPDATA%\Programs\PythonEmbed312\python.exe -m mkdocs serve`
> from this directory. If you later get a normal Python install working,
> switch to the standard `venv` flow above.

## Build

```bash
mkdocs build
```

Output goes to `site/` (git-ignored).

## Deployment — Cloudflare Pages

1. Push this repo to GitHub/GitLab.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**, select this repo.
3. Build settings:
   - **Build command:** `pip install -r requirements.txt && mkdocs build`
   - **Build output directory:** `site`
4. Deploy. Once the first deploy succeeds, go to the Pages project's
   **Custom domains** tab and add `doc.osamashalabi.com`, then follow
   Cloudflare's instructions to point the domain's DNS (a CNAME record) at
   the Pages project.
5. Every push to the main branch redeploys automatically.

## Content style

- Real technical content only — nothing invented. Ports, commands, and
  configuration steps come from official documentation or verified
  first-hand notes.
- Use admonitions for callouts: `!!! note`, `!!! warning`, `!!! tip`,
  `!!! important`.
- Use fenced code blocks with a language hint for syntax highlighting and
  the automatic copy button, e.g. ` ```bash `.
- Numbered steps for deployment procedures.
