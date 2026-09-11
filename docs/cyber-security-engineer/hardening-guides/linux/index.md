# Linux Hardening

Baseline hardening steps for a Linux host: locking down the boot process, encrypting data at rest, restricting network exposure, securing remote access, minimizing account privilege, keeping packages current, and making sure the system actually logs what happens on it.

| Topic | Description |
|---|---|
| [Physical Security](physical-security.md) | Password-protecting GRUB so the boot process can't be abused for unauthorized root access. |
| [File-System Partitioning & Encryption](filesystem-encryption.md) | Full-disk encryption with LUKS. |
| [Firewall](firewall.md) | Checking exposed ports with `ufw`. |
| [Remote Access & SSH Hardening](remote-access-ssh.md) | Defending against password sniffing/guessing and moving to key-based SSH authentication. |
| [Securing User Accounts](user-accounts.md) | sudoers, disabling root login, and disabling unused accounts. |
| [Update & Upgrade Policies](updates.md) | Keeping packages current on Debian-based and RHEL-based distributions. |
| [Audit & Log Configuration](audit-logging.md) | Where Linux keeps its logs, and how to search them quickly. |
