# Web Hacking — Shells

!!! warning "Authorized testing only"
    These are personal notes from authorized labs, CTFs (HTB/THM/picoCTF), and bug-bounty programs. Only use these techniques against systems you own or have explicit written permission to test.

A "shell" here means remote command execution — getting an interactive command line on the target machine over the network, instead of just being able to trigger one-off actions. There are two main shapes this takes, and the payload used to get one can be sent in different ways.

## Staged vs non-staged payloads

- **Non-staged**: sends the entire shellcode/exploit in one go — larger in size, and won't always work (e.g. if there's a size limit on the vulnerable input).
- **Staged**: sends the payload in stages (a small first-stage payload that then pulls down the rest) — can be less stable, but works around size limits.

## Reverse shell vs bind shell

- **Reverse shell** — the victim (target) connects *back* to the attacker. This is the most common choice when the target is behind NAT/a firewall that blocks inbound connections but allows outbound ones.
- **Bind shell** — the attacker connects *to* the target, which listens for the incoming connection. Useful when outbound connections from the target are blocked but a port can be opened for inbound access.

## Payload generation

- [revshells.com](https://www.revshells.com/) — the go-to site for generating reverse shell payloads in whatever language/format a target environment needs (PHP, Python, bash, etc.), for the attacker's IP/port.

## Where this gets used in practice

The [File Upload](file-upload.md) page walks through several real reverse-shell practical examples where the delivery mechanism was an insecure file-upload feature (a picoCTF challenge, a JetBrains CVE, and a generic upload-based foothold) — this page covers the underlying shell theory those practicals rely on.
