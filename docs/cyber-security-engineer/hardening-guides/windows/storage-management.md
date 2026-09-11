# Storage Management

## Data Encryption Through BitLocker

Microsoft's Business editions of Windows include BitLocker, a disk encryption feature that protects data at rest — if a drive is lost, stolen, or removed and attached to another system, its contents remain unreadable without the recovery key. On Windows 10 Home editions, a lighter "Device Encryption" variant of the same protection is also available.

Check whether it's enabled via `Start > Control Panel > System and Security > BitLocker Drive Encryption`.

## Windows Sandbox

Windows Sandbox is a temporary, isolated, lightweight desktop environment for safely running untrusted applications. Software installed inside it does not become part of the host machine — it stays sandboxed, and once the Sandbox is closed, everything (files, software, state) is deleted. This requires virtualization to be enabled on the OS.

Enable the feature via `Start > search "Windows Features" and turn it on > select Sandbox > click OK to restart`.

## Enable File Backups

Even with every other control in place, data loss can still happen — through malware, hardware failure, or human error. File backups are the last line of defense: if you lose essential data despite everything else, a backup is what actually lets you recover it.

Enable file backups via `Settings > Update and Security > Backup`.

!!! important
    Backups are only useful if they're tested. An untested backup is an assumption, not a recovery plan.
