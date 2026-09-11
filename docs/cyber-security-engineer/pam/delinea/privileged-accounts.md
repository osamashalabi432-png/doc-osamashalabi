# Delinea PAM — Privileged Accounts

## Protecting privileged accounts

The point of PAM is to stop privileged credentials from sitting around in plaintext or being shared informally — PAM safely stores and secures:

- Local admin passwords
- Domain admin credentials
- Service accounts
- Application and database credentials
- SSH keys
- API keys

Everything privileged that would otherwise be a standing secret (a password in a spreadsheet, a hardcoded API key) gets pulled into PAM instead, so it can be rotated, access-controlled, and audited rather than just known by whoever set it up.
