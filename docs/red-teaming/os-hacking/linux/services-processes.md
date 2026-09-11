# Linux — Services & Processes

!!! warning "Authorized lab material only"
    Techniques below were exercised against authorized HTB/CTF targets during study. Never run against systems you don't own or have written permission to test.

## Finding root-owned processes with exposed debug/admin ports

- **In plain terms:** some services (monitoring tools, language debuggers, admin panels) run as root but bind a control interface that has no authentication of its own — it only "protects" itself by being on localhost. Any local user, even unprivileged, can reach that interface and use it to make the root process do things on their behalf.
- **Why check `sudo -l` first, then processes:** `sudo -l` is the fastest privesc check — if it comes back empty/denied, the next step is to see what's actually running on the box and who owns it, since a root-owned service with an open management interface is a common second path to root.

```bash
sudo -l
ss -tlnp
ps aux
```

- `ss -tlnp` lists listening TCP ports and the owning process — look for anything unusual bound to `localhost` and note which user owns the process.
- `ps aux` cross-references process ownership — a process owned by `root` with a debug/inspector flag in its command line is the signal to chase.

**Example:** on HTB **Reactor**, this combination (`sudo -l` denied, then `ss -tlnp` + `ps aux`) revealed a root-owned Node.js process with its V8 Inspector debug port (`9229`) exposed on localhost. See [Privilege Escalation → Root process abuse via Node.js Inspector protocol](privilege-escalation.md#root-process-abuse-via-nodejs-inspector-protocol) for how that was exploited, and [Labs → Reactor](labs.md#reactor-nodejs-rce-root-process-abuse) for the full chain.

## Samba enumeration (HTB Kenobi)

- **In plain terms:** Samba (SMB) is a Linux implementation of the Windows file-sharing protocol. Enumerating it means listing what shares exist and what's readable/writable without credentials — a common first foothold source, since shares are sometimes left open or contain leftover config/credential files.
- On HTB **Kenobi**, the intended path started with enumerating Samba for shares, then moved on to a vulnerable ProFTPD version, then privilege escalation via `PATH` variable manipulation (see [Privilege Escalation → PATH variable manipulation](privilege-escalation.md#path-variable-manipulation-kenobi-htb)).
- Initial nmap scan against the box surfaced 6 open ports (Samba/NetBIOS among them).

!!! note "Source completeness"
    The Samba/ProFTPD enumeration steps for this box exist as screenshots in the original notes without transcribed command text. See [Labs → Kenobi](labs.md#kenobi) for what was captured in writing.
