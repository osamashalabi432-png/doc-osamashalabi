# Update & Upgrade Policies

Unpatched packages are one of the most common ways systems get compromised — publicly known vulnerabilities in outdated software give attackers a reliable, low-effort way in. Keeping the OS and installed packages current closes off that path.

## Debian-based distributions (e.g. Ubuntu)

```bash
apt update   # download package information from the configured sources
apt upgrade  # install available upgrades for all packages from the configured sources
```

## RHEL / Fedora-based distributions

```bash
dnf update   # newer releases (Red Hat Enterprise Linux 8 and later)
yum update   # older releases (Red Hat Enterprise Linux 7 and earlier)
```

!!! tip
    `update` refreshes the package index; `upgrade` actually installs the newer packages. Running `update` alone will not patch anything — both steps are required.
