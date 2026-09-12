# doc-osamashalabi

Personal cybersecurity documentation — enterprise security product
deployments, implementation notes, commands, troubleshooting, labs, and
red teaming research. Built with
[MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

**Live at: [docs.osamashalabi.com](https://docs.osamashalabi.com)**

Notes are written in [Obsidian](https://obsidian.md) against `docs/` as the
vault and synced to this repo via git — this repo is the single source of
truth for the published site.

## Structure

```
docs/
  index.md                       Home page (two entry cards)
  cyber-security-engineer/       Enterprise security products, by project category (not vendor)
  red-teaming/                   AI, web application, and OS offensive-security research
  stylesheets/extra.css          Minimal theme overrides
mkdocs.yml                       Site config, theme, nav, plugins
requirements.txt                 Pinned Python deps
```

Cyber Security Engineer is organized by **project category → product →
pages** (e.g. `firewall/fortigate/`, `pam/one-identity/`), never by vendor.
Red Teaming is organized by discipline (`ai-red-teaming/`, `web-hacking/`,
`os-hacking/{linux,windows}/`). The exact category and product list is
always current in `mkdocs.yml`'s `nav:`, which is the real source of truth
for what's on the site.

Pages carrying a `!!! note "Placeholder"` callout haven't been written yet.
Nothing on this site is invented — architecture details, commands, ports,
and licensing info are only added once verified against official docs or
first-hand deployment/lab notes.

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

## Build

```bash
mkdocs build
```

Output goes to `site/` (git-ignored).

## Deployment — Cloudflare Pages

The site is deployed as a Cloudflare Pages project (`doc-osamashalabi`)
with `docs.osamashalabi.com` attached as a custom domain over the
`osamashalabi.com` Cloudflare zone.

Recommended setup for continuous deployment on push:

1. In the Cloudflare dashboard: **Workers & Pages → Create → Pages →
   Connect to Git**, select this repo.
2. Build settings:
   - **Build command:** `pip install -r requirements.txt && mkdocs build`
   - **Build output directory:** `site`
3. Every push to the production branch redeploys automatically.

A build can also be deployed directly without Git integration:

```bash
mkdocs build
npx wrangler pages deploy site --project-name doc-osamashalabi
```

## Content style

- Real technical content only — nothing invented. Ports, commands, and
  configuration steps come from official documentation or verified
  first-hand notes.
- Use admonitions for callouts: `!!! note`, `!!! warning`, `!!! tip`,
  `!!! important`.
- Use fenced code blocks with a language hint for syntax highlighting and
  the automatic copy button, e.g. ` ```bash `.
- Numbered steps for deployment procedures.
