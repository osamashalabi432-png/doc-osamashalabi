# Docker — Hardening

Practical measures for reducing Docker's attack surface, following on from the concrete vulnerabilities covered in [Security](security.md).

## Protecting the Docker daemon

### Docker over SSH with Docker contexts

Developers often need to manage Docker on **remote machines**. Docker supports doing this over **SSH** instead of exposing the daemon's API on the network (the mistake behind [Vulnerability #3](security.md#vulnerability-3-rce-via-an-exposed-docker-daemon-api)).

Docker uses **contexts** (think of them as connection profiles) to store remote connection configs, so you can switch between environments (e.g. *dev* vs *prod*) without re-typing connection details each time.

**Prerequisites:**

- SSH access to the remote host.
- The remote user must be allowed to run Docker commands (e.g. be in the `docker` group or have equivalent permissions).

**Workflow:**

1. Create a context pointing at the remote Docker daemon over SSH:

    ```bash
    docker context create --docker host=ssh://user@remotehost ...
    ```

2. Switch to that context — after this, all Docker commands run against the remote host:

    ```bash
    docker context use <context-name>
    ```

3. Switch back to local Docker when done:

    ```bash
    docker context use default
    ```

!!! warning "SSH security is the actual security boundary here"
    SSH transport is encrypted, but the setup is only as strong as the SSH configuration behind it. Weak passwords can lead to compromise — use strong passwords (long, mixed-character) and good SSH hygiene.

### cgroups, resource limits, and namespaces

**cgroups (Control Groups)** are a Linux kernel feature that lets you limit, prioritize, and track how much CPU/RAM a process can use. In Docker, cgroups help isolate containers by enforcing resource caps — a second line of defense if a container misbehaves, preventing one buggy or malicious container from consuming all of the host's resources and taking the whole system down.

Resource limits are **not enabled by default** — they're set per container at run time:

```bash
# CPU limit
docker run --cpus="1" ...

# Memory limit (supports k/m/g suffixes)
docker run --memory="20m" ...
```

Limits can be changed on a running container:

```bash
docker update --memory="40m" mycontainer
```

And viewed with:

```bash
docker inspect <container>
```

If the reported values are `0`, it usually means no limit is set.

**Namespaces** are a separate Linux kernel feature Docker relies on for isolation — think of them as separate "rooms," so that actions inside one container/process don't affect others.

### Privileged containers and capabilities

Rather than running a container with the broad `--privileged` flag, it's recommended to assign individual Linux **capabilities** to a container only as needed. `--privileged` effectively disables most of the isolation Docker provides ([Vulnerability #1](security.md#vulnerability-1-privileged-containers) shows exactly what that makes possible).

To see what capabilities are currently assigned to a process:

```bash
capsh --print
```

### Image review

Always review the contents of a Docker image before running it — treat pulled images the same way you'd treat any other third-party software.

## Compliance scanners

Several tools can assess whether a Docker deployment complies with recognized security benchmarks (CIS Docker Benchmark, NIST SP-800-190, etc.):

| Benchmarking tool | Description | URL |
|---|---|---|
| CIS Docker Benchmark | Assesses a container's compliance with the CIS Docker Benchmark framework. | [cisecurity.org/benchmark/docker](https://www.cisecurity.org/benchmark/docker) |
| OpenSCAP | Assesses compliance with multiple frameworks, including CIS Docker Benchmark and NIST SP-800-190. | [open-scap.org](https://www.open-scap.org/) |
| Docker Scout | Docker's own cloud-based scanning service for images and libraries; lists vulnerabilities and provides remediation steps. | [docs.docker.com/scout](https://docs.docker.com/scout/) |
| Anchore | Assesses compliance with multiple frameworks, including CIS Docker Benchmark and NIST SP-800-190. | [github.com/anchore/anchore-engine](https://github.com/anchore/anchore-engine) |
| Grype | A modern, fast vulnerability scanner for Docker images. | [github.com/anchore/grype](https://github.com/anchore/grype) |

### Practical scanning with Grype

Scan a Docker image for vulnerabilities across all layers:

```bash
grype imagename --scope all-layers
```

Scan an exported container filesystem (e.g. produced by `docker image save`):

```bash
grype /path/to/image.tar
```
