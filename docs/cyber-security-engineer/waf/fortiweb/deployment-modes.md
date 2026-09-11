# FortiWeb — Deployment Modes

How FortiWeb is inserted into the traffic path changes what it can see and how it protects the servers behind it. FortiWeb supports five deployment/blocking modes:

| Mode | How it works |
|---|---|
| **Reverse Proxy** | FortiWeb acts as an intermediary between clients and your web servers. It terminates the client's session, inspects the traffic, and then creates a *new* connection to the backend server. |
| **Offline Protection** | FortiWeb sits *out of band*, passively monitoring mirrored traffic without being inline. |
| **True Transparent Proxy** | FortiWeb is inline but *transparent* to clients and servers — it does not change IPs. |
| **Transparent Inspection** | Similar to true transparent, but FortiWeb only *inspects* packets rather than proxying them. |
| **WCCP (Web Cache Coordination Protocol)** | FortiWeb integrates with a router or cache server that *redirects* traffic to it for inspection. |

!!! important
    FortiWeb should always be deployed behind a firewall — it isn't designed to protect against non-HTTP attacks.

## Setup scenarios

### Traffic mirroring

Traffic mirroring lets FortiWeb send a copy of the traffic it sees to third-party IPS/IDS devices, so those devices can perform their own real-time monitoring, threat detection, or forensic analysis without sitting inline themselves.

Mirroring support depends on the proxy mode:

| Mode | Mirroring support |
|---|---|
| **Reverse proxy mode** | Supported on both physical and virtual FortiWeb appliances. |
| **True transparent proxy mode** | Supported on virtual FortiWeb only. |

**Connection types** — how the mirrored traffic physically/logically reaches the IPS/IDS:

| Connection Type | Description |
|---|---|
| **Physical port** | Direct connection between FortiWeb and the IPS/IDS device (simplest setup). |
| **Through a switch** | Traffic is sent via a network switch — requires a destination MAC address so it knows where to deliver mirrored packets. |
| **Through the network** | The IPS/IDS listens over the network in server mode, so mirrored traffic is sent using IP and port, like normal packets. |

**Modes of traffic mirroring** — how FortiWeb sends the mirrored data:

| Mode | How it works | Used when |
|---|---|---|
| **Direct mode** | Traffic is sent straight from a FortiWeb port to the IPS/IDS device. | The IPS/IDS is physically connected. |
| **Switch mode** | Traffic goes through a switch using a destination MAC address. | Multiple devices share the same network switch. |
| **Server mode** | Traffic is sent to a specific IP and port, like normal network communication. | The IPS/IDS operates as a server receiving mirrored packets. |

### Deploying behind a load balancer / ADC

When FortiWeb sits behind another device that terminates and re-originates connections (such as a FortiADC), every request FortiWeb sees can end up appearing to come from a single source IP (the ADC's address) instead of the real client. This causes two problems:

1. **FortiWeb can't identify or log the real client.** Security rules, rate limiting, and reputation filtering depend on the client IP — but if every connection looks like it's from the same address, per-client visibility is lost, logs all show the same source IP, and source-based analytics become useless.
2. **FortiWeb may block the ADC itself.** FortiWeb's DoS protection or IP reputation engine can conclude that the ADC's IP is an attacker, since *all* requests appear to originate from it — and end up blocking the ADC, breaking access for every user behind it.

This is exactly the scenario FortiWeb's **XFF Header Rules** policy exists for — it controls how FortiWeb reads the `X-Forwarded-For` header so it can correctly identify the real client IP when sitting behind a load balancer or ADC, instead of relying on the TCP source address alone (see [Policies](policies.md)).
