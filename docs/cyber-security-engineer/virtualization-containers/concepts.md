# Virtualization & Containers — Concepts

Before getting into specific tools, it helps to understand *why* virtualization exists and the two different approaches to it: hypervisor-based virtual machines and OS-level containers.

## What Is Virtualization

Virtualization is the concept of taking the capabilities and features of a physical machine and encapsulating them into a virtual environment — a virtual machine (VM) — so that a single physical host can run multiple isolated "machines" at once.

Organizations adopt virtualization mainly for three reasons:

- **Decreased expenses** — physical servers are expensive. Virtualization reduces the number of physical servers (or other hardware) a company needs, and can remove physical hardware from the infrastructure entirely.
- **Scale** — without proper DevOps practices, it can be hard to scale resources as demand grows. Virtualization makes it easier to delegate a server's resources to VMs on demand, based on usage.
- **Efficiency** — the same flexibility works in reverse: it's easier to scale *down* the resources allocated to a VM when usage drops.

## Hypervisors

A **hypervisor** creates the abstraction layer between physical hardware and the software running on top of it. It also generally ships with a management application that gives an end user an interface to create and control virtual machines through that abstraction layer.

Hypervisors fall into two categories, based on where they sit relative to the hardware.

| Type | Also known as | How it works | Examples |
|---|---|---|---|
| **Type 1** | Bare-metal hypervisor | Runs directly on the hardware as the operating system itself — no general-purpose OS underneath. Often headless, managed through a web portal. Designed to be lightweight and to run many VMs at scale. | VMware ESXi, Proxmox, VMware vSphere, Xen, KVM |
| **Type 2** | Hosted hypervisor | Runs as an application on top of a pre-existing operating system, usually managed through a desktop GUI. Aimed at end users/developers rather than large-scale VM hosting. | VMware Workstation, VMware Fusion, VirtualBox, Parallels, QEMU |

!!! note "Why the distinction matters"
    Type 1 hypervisors trade convenience for performance and scale — there's no host OS competing for resources, so more capacity goes to the VMs. Type 2 hypervisors trade some of that performance for convenience: they install like a normal application on a machine you're already using.

## Containers

Containers are the modern answer to the overhead and scaling limits of running everything in full VMs.

Unlike a VM, a container is not completely abstracted from the host operating system — it shares the host's kernel. Each container gets its own filesystem, its own slice of compute resources (CPU/RAM), and its own process space, but it isn't a full separate OS. That's what makes containers lightweight, portable, and quick to start compared to VMs.

Where a hypervisor provides the abstraction layer for virtual machines, a **container engine** (e.g. Docker, Podman) provides the equivalent abstraction layer for containers, using logical/OS-level resource isolation instead of hardware-level abstraction.

## Docker and Kubernetes, at a glance

Two tools come up constantly once you start working with containers:

- **Docker** is a container platform and engine used to build and run container images. Each image is built from a lightweight base image (e.g. Alpine, Ubuntu) plus a set of build instructions defined in a `Dockerfile`. Images are commonly distributed through **Docker Hub**, a remote image registry (comparable to how GitHub hosts Git repositories) — `docker pull` fetches an image from it, and `docker run` will pull it automatically on first use if it isn't already cached locally.
- **Kubernetes** ("K8s") is an **orchestration platform**. Rather than replacing Docker or hypervisors, it integrates with and extends them — coordinating many containers across many hosts. Its main value comes from:
    - **Horizontal scaling** — handling more load by adding more machines, instead of adding more CPU/RAM to one machine (vertical scaling).
    - **Extensibility** — clusters can be modified dynamically without disrupting containers outside the affected group.
    - **Self-healing** — Kubernetes can restart, replace, reschedule, or kill containers that fail user-defined health checks.
    - **Automated rollouts and rollbacks** — changes are rolled out progressively while application health is monitored, with automatic rollback if something goes wrong, to keep the cluster available even when some containers fail.

See the [Docker](docker/index.md) and [Kubernetes](kubernetes/index.md) sections for the command-level detail and hardening/security notes.
