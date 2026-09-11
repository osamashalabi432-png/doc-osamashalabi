# Securing User Accounts

Logging in and operating as `root` for day-to-day administration is risky: every command runs with full system privileges, mistakes are unrecoverable, and any compromised session is a full compromise. The better approach is to create an account for administrative purposes and add it to the *sudoers* group, so privilege escalation is deliberate, scoped to individual commands, and logged.

Add a user to the sudo/wheel group:

```bash
usermod -aG sudo username

# for other distributions
usermod -aG wheel username
```

## Disable Root

Once you've created an administrative account and added it to the `sudo`/`wheel` group, consider disabling the `root` account outright so it can no longer be used to log in directly.

A straightforward way is to edit `/etc/passwd` and change the `root` shell to `/sbin/nologin`:

```text
# before
root:x:0:0:root:/root:/bin/bash

# after
root:x:0:0:root:/root:/sbin/nologin
```

## Disable Unused Accounts

Stale accounts (former employees, decommissioned service accounts, default accounts nobody uses) are an easy target — nobody notices if they're compromised because nobody is watching them. Disable a user account the same way as the root account: edit `/etc/passwd` and set that user's shell to `/sbin/nologin`.

For example, to disable the account of user Michael (`michael`):

```text
# enabled account
michael:x:1000:1000:Michael:/home/michael:/usr/bin/fish

# disabled account
michael:x:1000:1000:Michael:/home/michael:/sbin/nologin
```

!!! tip
    Periodically audit `/etc/passwd` for accounts that no longer need shell access — this is cheap to do and closes off accounts that would otherwise sit unnoticed as an attack path.
