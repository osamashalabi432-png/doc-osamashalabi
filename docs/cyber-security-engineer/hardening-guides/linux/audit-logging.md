# Audit & Log Configuration

Hardening prevents some attacks, but detection depends entirely on what gets logged and whether anyone looks at it. Most log files on Linux systems are stored in the `/var/log` directory.

## Common Log Locations

| Log file | Contents |
|---|---|
| `/var/log/messages` | General log for Linux systems |
| `/var/log/auth.log` | All authentication attempts (Debian-based systems) |
| `/var/log/secure` | All authentication attempts (Red Hat and Fedora-based systems) |
| `/var/log/utmp` | Users currently logged into the system |
| `/var/log/wtmp` | All users that have logged in and out of the system |
| `/var/log/kern.log` | Messages from the kernel |
| `/var/log/boot.log` | Start-up messages and boot information |

## Working With Large Log Files

Two commands cover most day-to-day log review needs:

- Since new events are appended to the log file, view the last few lines with `tail`. For example:

```bash
tail -n 12 boot.log
```

displays the last 12 lines.

- Search log lines for a specific keyword with `grep`. For example:

```bash
grep FAILED boot.log
```

shows only the lines containing the word `FAILED`.

!!! tip
    `/var/log/auth.log` (or `/var/log/secure`) combined with `grep` for failed logins is usually the fastest way to spot brute-force or password-guessing attempts against a host.
