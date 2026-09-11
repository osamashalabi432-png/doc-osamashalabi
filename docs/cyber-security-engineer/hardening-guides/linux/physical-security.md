# Physical Security — GRUB Password

An attacker who has physical (or console) access to a Linux machine can interrupt the boot process and reach the GRUB bootloader menu. From there, without any further authentication, they can edit boot parameters to drop into a root shell — completely bypassing the normal login and any account-level restrictions on the system.

Setting a GRUB password closes that gap: it requires a password to access advanced boot configurations, including anything that would grant root access via GRUB.

!!! important "Why this matters"
    Disk encryption, sudoers restrictions, and SSH hardening are all irrelevant if an attacker can simply reboot the box and edit kernel boot parameters to spawn a root shell. GRUB hardening is the first link in the chain.

Generate a hashed password for GRUB with:

```bash
grub2-mkpasswd-pbkdf2
```

This produces a PBKDF2 hash that gets placed into the GRUB configuration so that the boot menu — and any editing of boot entries — requires that password before proceeding.
