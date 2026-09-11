# General Concepts

Understanding Active Directory's logical structure is a prerequisite for hardening it — each layer (domain, tree, forest) defines a trust boundary, and hardening decisions (like the Tiered Access Model) are built directly on top of these boundaries.

## Domain

The domain is the core unit of Active Directory's logical structure. It stores all the critical information about the objects that belong to that domain only.

## Domain Controller

A Domain Controller is an Active Directory server that acts as the "brain" for a Windows server domain — it supervises the entire network. Within the domain, it acts as the gatekeeper for user authentication and IT resource authorization. Because of this, Domain Controllers are the highest-value target in an AD environment and are treated as Tier 0 assets (see [Least Privilege & Tiering](least-privilege-tiering.md)).

## Trees

Trees share resources between domains. Communication between domains inside a tree happens via either a one-way or two-way trust. When a domain is added to a tree, it becomes the offspring (child) domain of the domain it was added to — which becomes its parent domain.

## Forests

A forest is formed when a collection of trees successfully shares a common global catalogue, directory schema, logical structure, and directory configuration. Communication between two separate forests becomes possible once a forest-level trust is created.

Access AD trust configuration via:

```text
Server Manager > Tools > Active Directory Domains and Trust
```
