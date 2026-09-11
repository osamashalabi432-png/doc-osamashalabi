# Remote Access & SSH Hardening

Remote access is one of the most commonly attacked surfaces on any Linux system. Common attack patterns include:

1. Password sniffing
2. Password guessing and brute-forcing
3. Exploiting the listening service itself

## Protecting Against Password Sniffing

Remote access can be achieved through many different protocols and services. Although all modern systems use encrypted protocols such as SSH for remote access, older systems might still use cleartext protocols such as Telnet — which transmit credentials in the clear and can be captured by anyone able to observe the traffic.

## Protecting Against Password Guessing

A few guidelines significantly reduce the risk of password guessing and brute-forcing:

1. Disable remote login as `root`; force login as non-root users.
2. Disable password authentication; force public key authentication instead.

!!! important "Why disable root login"
    If `root` login is allowed remotely, an attacker only needs to guess one username. Forcing login as a named, non-root user means an attacker also has to guess (or already know) a valid username before they can even attempt a password, and any successful administrative action still has to go through `sudo` — which is logged.

The OpenSSH server is configured via `sshd_config`, usually located at `/etc/ssh/sshd_config`. Disable root login by adding:

```bash
PermitRootLogin no
```

### Moving to Key-Based Authentication

Relying on public key authentication with SSH — instead of passwords — substantially improves the security of remote login, since there's no password to guess, sniff, or brute-force.

If you haven't already created an SSH key pair, generate one with:

```bash
ssh-keygen -t rsa
```

This generates a private key (`id_rsa`) and a public key (`id_rsa.pub`).

For the SSH server to authenticate you using your public key instead of a password, the public key needs to be copied to the target server. The easiest way to do this is:

```bash
ssh-copy-id username@server
```

where `username` is your username and `server` is the hostname or IP address of the SSH server.

!!! warning "Don't lock yourself out"
    Make sure you have access to the physical terminal (or an already-working key-based session) before disabling password authentication. Losing password access before key-based access is confirmed working can lock you out of the box entirely.

Once key-based login is confirmed working, ensure the following two lines are set in `sshd_config`:

```bash
PubkeyAuthentication yes
PasswordAuthentication no
```

- `PubkeyAuthentication yes` — enables public key authentication.
- `PasswordAuthentication no` — disables password authentication, closing off password-guessing attacks entirely.
