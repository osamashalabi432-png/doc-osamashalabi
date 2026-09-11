# Palo Alto — SASE / Prisma Access

Secure Access Service Edge (SASE) is an architecture that combines networking and security into a single cloud-delivered platform, rather than stitching together separate on-premises appliances for each function. Palo Alto's implementation of this is **Prisma SASE / Prisma Access**.

## The problems SASE solves

SASE exists to address four core problems with traditional, appliance-based network security:

1. **Non-agile operations** — traditional infrastructure is slow to change or scale.
2. **Inconsistent security posture** — policy enforcement varies depending on where a user or device connects from.
3. **High operational cost and complexity** — maintaining separate hardware stacks per site is expensive and hard to manage.
4. **Poor user experience** — backhauling traffic through central appliances adds latency for end users.

> Traditional security is too slow, too fragmented, too expensive, and too frustrating for users.

Prisma SASE combines networking and security into one cloud-delivered platform to address these problems directly.

## Prisma Access

Prisma Access protects hybrid workers by giving them secure, direct access to applications, with continuous trust checks and continuous security inspection applied to their traffic — rather than a one-time login check.

## Infrastructure terms

Four Prisma Access infrastructure terms are easy to confuse with each other: **Locations**, **Compute Locations**, **Nodes**, and **Regions**. The easiest way to keep them straight is to think of them as layers, from broadest to most specific:

| Term | Meaning |
|---|---|
| **Region** | A big logical area. |
| **Location** | A Prisma Access site in a specific geographic place. |
| **Compute Location** | The data center where processing actually happens. |
| **Node** | The actual connection/processing point used by traffic. |

## Service Infrastructure

The Service Infrastructure subnet acts as a private backbone for Prisma Access — without it, Prisma Access has no internal address space to work with.

| Component | Connects How | Main Role |
|---|---|---|
| **GlobalProtect VPN** | Portal/Gateway public IPs | Connects mobile users |
| **Remote Networks** | IPSec tunnel to Service IP | Connects branch sites |
| **Data Center Applications** | Service connection | Gives access to HQ/internal apps |
| **Secure Channel Protocol** | Encrypted channels between components | Protects internal communications |

## Service Connections

Service Connections give Prisma Access three capabilities:

1. Allow users access
2. Allow users to communicate
3. Improve network efficiency

### Primary and secondary tunnels

- The **first tunnel** created for a service connection is the **primary tunnel**. **Tunnel Monitoring** can be enabled so Prisma Access can check whether it's up.
- Prisma Access also supports a **secondary tunnel** for redundancy:
    - If both tunnels are up, the **primary tunnel is preferred**.
    - If the primary fails, traffic **fails over to the secondary**.
    - When the primary comes back up, it becomes active again.
