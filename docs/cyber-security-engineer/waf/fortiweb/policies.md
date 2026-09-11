# FortiWeb — Policies

A FortiWeb **server policy** is the object that ties together where traffic enters, where it goes, and what protection is applied to it. It's built from three main components:

1. **Virtual Server** — the front-end interface on FortiWeb where clients connect. For example, `https://www.company.com` might point to FortiWeb's public IP; this is the "entry point" for client requests.
2. **Server Pool** — defines the backend web servers FortiWeb sends traffic to after inspection (for example, Web Server 1 and Web Server 2 in the same pool). FortiWeb distributes or proxies traffic to these servers.
3. **Web Protection Profile** — the security policy applied to traffic handled by that server policy. It defines which protections are enabled (SQL injection detection, XSS filtering, etc.), and its exact contents vary depending on whether FortiWeb is operating in **reverse proxy**, **true transparent**, or **offline sniffing** mode (see [Deployment Modes](deployment-modes.md)).

## Policy types

Beyond the core server policy, FortiWeb supports several narrower policy types that plug into a web protection profile:

| Policy Type | Purpose |
|---|---|
| **XFF Header Rules** | Controls how FortiWeb reads the `X-Forwarded-For` header, to correctly identify the client IP — especially when FortiWeb sits behind load balancers or ADCs. |
| **URL Access Policies** | Define which URLs are allowed or blocked (for example, restricting access to `/admin` pages). |
| **Geolocation IP Policy** | Restrict or allow access based on the geographic location of the client IP (for example, blocking traffic from specific countries). |
| **Web Shell Detection Policy** | Detects and blocks hidden malicious scripts (web shells) that attackers upload to compromise servers. |
