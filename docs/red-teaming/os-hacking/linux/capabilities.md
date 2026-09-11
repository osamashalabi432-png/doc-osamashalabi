# Linux — Capabilities

!!! warning "Authorized lab material only"
    Techniques below were exercised against authorized HTB/CTF targets. Never run against systems you don't own or have written permission to test.

## Linux capability abuse — `cap_setuid` on python3.8

- **In plain terms:** Linux "capabilities" split up root's power into individual pieces (e.g. binding to a low port, reading raw network packets) so a program can get exactly one root-level power without being fully root. `cap_setuid` is the power to become *any* user, including root — so if it's mistakenly granted to a general-purpose program like the Python interpreter, anyone who can run Python on the box can just ask to become root, no exploit code needed.
- Root cause: `/usr/bin/python3.8` had the `cap_setuid,cap_net_bind_service+eip` capabilities set — likely left over from granting a web app raw-socket/bind access for a `tcpdump` feature.
- `cap_setuid+eip` lets **any process spawned through that specific binary** call `setuid(0)` directly — no `sudo`, no SUID bit needed.
- **Why check capabilities at all:** `sudo -l` and SUID binaries are the usual first privesc checks; capabilities are a lesser-known third place root-level power can hide, so they're worth checking whenever the usual routes come up empty.

- Enumerate all capability-bearing binaries on the box:

```bash
getcap -r / 2>/dev/null
```

- **Why this command works as an exploit:** `os.setuid(0)` is a normal Python standard-library call — it only succeeds because the *interpreter binary itself* carries the `cap_setuid` power. Once the process's UID switches to `0`, everything it does afterward (like spawning `/bin/bash`) runs as root.

- Exploit is a one-liner once found:

```bash
python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

- Proof (from the **Cap** HTB box — see [Labs → Cap](labs.md#cap-idor-credential-leak-python-cap_setuid-privesc) for the full chain):

```bash
$ python3.8 -c 'import os; os.setuid(0); os.system("id; cat /root/root.txt")'
uid=0(root) gid=1001(nathan) groups=1001(nathan)
b3973a0e0cc92a98a07f70896d1f6375
```

!!! tip "Recommendations"
    - Remove unnecessary capabilities: `setcap -r /usr/bin/python3.8`.
    - If an app genuinely needs raw-socket/bind capabilities, grant them to a dedicated binary/venv interpreter, never a shared system-wide interpreter every user can invoke.
    - Audit capabilities regularly: `getcap -r / 2>/dev/null`.

**Full machine walkthrough:** this technique was the privesc step on HTB **Cap** (IDOR credential leak + `cap_setuid` privesc) — see [Labs → Cap](labs.md#cap-idor-credential-leak-python-cap_setuid-privesc) for the complete chain, including the FTP/SSH foothold that came before it.
