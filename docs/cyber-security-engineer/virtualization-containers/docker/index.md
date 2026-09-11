# Docker — Overview

Docker is a container platform and engine used to build, run, and ship containerized applications. It runs Docker "images" as containers — a running instance of an image, isolated from the host OS at the process/filesystem level rather than fully virtualized like a VM.

Each Docker image is built from a base image (commonly something lightweight like Alpine or Ubuntu, purpose-built for containers) plus a set of instructions written in a **Dockerfile** — a plain-text file listing every command needed to assemble the image, starting with a `FROM` instruction that defines which base OS/image to build on top of.

Images are typically pulled from **Docker Hub**, a remote image registry (the container-world equivalent of GitHub for Git repositories). Once an image has been pulled once, Docker caches it locally and checks the local cache before trying to download it again.

## In this section

| Page | Description |
|---|---|
| [Commands](commands.md) | Core Docker CLI reference — inspecting containers/images, networking, and Docker Compose. |
| [Security](security.md) | Three real-world Docker container-escape and RCE vulnerabilities, with attack breakdowns. |
| [Hardening](hardening.md) | Protecting the Docker daemon, resource limits, privileged containers, image review, and compliance scanners. |
| [Tools](tools.md) | Notable Docker security tooling. |

!!! tip "Related reading"
    See [Podman](../podman/index.md) for a Docker-CLI-compatible alternative, and [Kubernetes](../kubernetes/index.md) for orchestrating Docker containers at scale.
