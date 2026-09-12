# Linux — Privilege Escalation

## Root process abuse via Node.js Inspector protocol

- **In plain terms:** Node.js has a built-in remote debugger (the "Inspector" / V8 debug protocol). If a Node process is started with that debugger enabled and running as `root`, anyone who can reach the debug port can attach and run arbitrary commands *as root* — the debugger was never designed to be a security boundary.
- Root cause: a Node.js worker was running as root with the V8 Inspector/debug protocol bound to `localhost`, port `9229`, debugging `/opt/uptime-monitor/worker.js`.
- The inspector protocol has no authentication beyond the bind address, so any local user can attach.
- **Why enumerate ports/processes here:** the goal is to spot an exposed debug port on a root-owned process — enumerate listening ports and running processes, then confirm via the inspector's JSON endpoint on that port.

```bash
node inspect 127.0.0.1:9229
```

- Attach with Node's built-in CLI debugger client, which drops into an interactive `debug>` REPL.
- Inside that REPL, a bare `require` is not defined — go through `process.mainModule.require` instead to reach Node's process-spawning module, then call its synchronous exec method to run arbitrary commands.
- Checking `process.getuid()` first confirms the debugged process runs as uid `0` (root); the exec call then returns command output as root — no interactive root shell needed.

!!! tip "Recommendations"
    - Never expose the inspector/debug flag for a process running as root, even bound to `localhost`, on a multi-user host.
    - Run monitored services as an unprivileged user.
    - If debugging is required, restrict access further (network namespace or firewall) and keep sessions short-lived.

**Full machine walkthrough:** this was the privesc step on HTB **Reactor** (Node.js RCE foothold + root process abuse) — see [Labs → Reactor](labs.md#reactor-nodejs-rce-root-process-abuse) for the complete chain, including how the foothold was obtained. See also [Services & Processes](services-processes.md) for the `ss -tlnp` / `ps aux` enumeration that surfaces this kind of exposed debug port.

## PATH variable manipulation (Kenobi, HTB)

- **In plain terms:** when a privileged program calls another program by name instead of by full path (e.g. `nmap` instead of `/usr/bin/nmap`), it relies on the shell's `PATH` variable to find it. If an attacker can control `PATH` (or drop a malicious binary earlier in the search order) before that privileged program runs, they can trick it into executing attacker-controlled code with elevated privileges instead of the real one.
- On HTB **Kenobi**, privilege escalation was achieved by manipulating the `PATH` environment variable so that a program executed by a higher-privileged context resolved to an attacker-controlled binary instead of the legitimate one.

!!! note "Source completeness"
    The original notes for this box are mostly screenshots (nmap results, Samba enumeration, ProFTPD exploitation) without transcribed command text, so the step-by-step commands for the ProFTPD exploit and the exact PATH-hijack sequence aren't reproduced here verbatim. See [Labs → Kenobi](labs.md#kenobi) for everything that was captured in writing.

## See also

- [CVE Writeups](cve-writeups.md) — Dirty Pipe (CVE-2022-0847) and Pwnkit (CVE-2021-4034), two kernel/Polkit local privesc bugs.
- [Capabilities](capabilities.md) — `cap_setuid` abuse on `python3.8` (HTB **Cap**).
- `gtfobins.github.io` — reference for privesc via misconfigured binaries/permissions once a foothold is established.
