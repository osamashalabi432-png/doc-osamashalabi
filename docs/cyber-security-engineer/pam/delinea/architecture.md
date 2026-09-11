# Delinea PAM — Architecture

## Reverse proxy placement

A **reverse proxy** is a server that sits in front of one or more internal servers: it receives requests from clients (browsers/agents), forwards them to the internal web server, and sends the response back to the client.

Where it fits with Delinea / Privilege Manager:

```text
Agent / Browser → Reverse Proxy → Privilege Manager Server
```

### Why put one in front of Privilege Manager?

1. **Security / segmentation**
    - Keeps the PAM server off the public internet.
    - Only the reverse proxy is exposed in the DMZ / public zone.
    - Hides real server names/IPs and internal topology.
2. **Single stable URL**
    - Lets you expose something like `https://pam.company.com` regardless of what's behind it.
    - Useful when there are multiple back-end nodes, or for HA / load balancing.
3. **SSL / certificates**
    - SSL can be terminated (or offloaded) at the proxy.
    - One public certificate on the proxy instead of managing certs on every backend node.
4. **Load balancing / HA**
    - The proxy can distribute traffic across multiple Privilege Manager servers.
    - Can perform health checks and failover.
5. **Additional security features**
    - WAF (Web Application Firewall)
    - IP filtering / geo-blocking
    - Rate limiting, etc.

In short: the reverse proxy is what lets the PAM server itself stay off the public internet while still being reachable at a single, stable, properly-secured URL.
