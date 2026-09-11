# Microsoft Azure — Networking — Virtual Networks

## What a VNet is

A Virtual Network (VNet) establishes a private, isolated network environment inside Azure. It's the cloud equivalent of a traditional on-premises LAN — a **data center network in the cloud** — and it's the foundation almost everything else in an Azure environment sits on top of.

Concretely, a VNet:

- Provides a **secure virtual network** where Azure resources (VMs, databases, services) can communicate with each other.
- Lets the team **control IP addressing, subnets, routing, and security** for those resources, instead of relying on defaults.
- Enables **incremental migration** — workloads can be moved into Azure step by step without disrupting systems that haven't moved yet.
- Forms the **foundation** for resources that get layered on top of it later, such as:
    - Virtual Machines
    - Subnets
    - Network Security Groups (NSGs)
    - VPN or ExpressRoute connections

!!! note
    A VNet by itself doesn't do anything — it's the addressable space that other resources (VMs, subnets, NSGs) get placed into. Creating a VNet is typically one of the first steps when standing up new infrastructure in Azure.

## IP addressing (CIDR)

When a VNet is created, it's given an IPv4 address range in CIDR notation — e.g. `192.168.0.0/24` or `10.0.0.0/16`. That range defines the pool of private IP addresses available to anything placed inside the VNet (VMs, subnets, etc.). If the exact range doesn't matter yet, Azure's default CIDR block can be accepted as-is; if the network needs to fit a specific addressing plan (matching an on-prem network, avoiding overlap with another VNet, sizing for how many hosts will live in it), a specific CIDR block is chosen instead.

## Subnets

A subnet divides a VNet's address space into smaller segments. Resources are placed into a subnet (not directly into the VNet), which allows different segments of the network to be managed, secured, and routed independently — for example, putting web servers in one subnet and databases in another, each with its own security rules.

For the step-by-step procedure used to create VNets and subnets in the Azure Portal, see [Labs](../labs.md).
