# Microsoft Azure — Overview

Microsoft Azure is Microsoft's public cloud platform. Instead of racking physical servers, infrastructure (virtual machines, networks, storage, IP addresses) is provisioned on demand through the Azure Portal, CLI, or PowerShell, and billed for what's actually used.

The content in this section comes from a set of hands-on labs that migrate a piece of infrastructure into Azure incrementally, rather than moving everything at once. That "small pieces, one at a time" approach is itself a deliberate migration strategy: it keeps each change small enough to control, gives a rollback point if something goes wrong, and lets the team validate one building block (an SSH key, a VM, a network) before depending on it for the next one.

The core building blocks covered here are:

- **Identity for access** — an SSH key pair used to authenticate into VMs instead of passwords.
- **Compute** — creating an Azure Virtual Machine (VM) with a chosen image, size, and disk.
- **Networking** — Virtual Networks (VNets) and subnets that give VMs a private, isolated address space, plus public IP addresses for reaching a VM from outside Azure.
- **Storage** — attaching an existing managed disk to a VM as additional data storage.
- **Network interfaces** — attaching an existing NIC, and attaching a public IP to that NIC, so a VM becomes reachable.

See [Labs](labs.md) for the numbered, step-by-step walkthrough of each task, and [Virtual Networks](networking/virtual-networks.md) for the networking concepts behind them.

!!! note
    This section only covers what was actually exercised in the labs. Broader Azure services (Entra ID, Defender, Monitor, deployment automation, etc.) aren't covered yet — those pages remain placeholders until there's verified first-hand content for them.
