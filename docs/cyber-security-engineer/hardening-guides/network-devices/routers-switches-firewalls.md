# Hardening Routers, Switches & Firewalls

Examples below reference OpenWrt's menu structure, but the underlying principles apply to router/switch/firewall administration generally.

## Change Default Credentials

The admin web interface is normally protected by a username and password, but default credentials are frequently left unchanged. A threat actor who finds a device still using default credentials can access the admin interface and compromise the whole network behind it.

Change the default password in OpenWrt via `System > Administration`, enter a new password, and click **Save**.

## Enable Secure Network Protocols

To maintain the confidentiality, integrity, and availability of network traffic, secure protocols must be enabled. Protocols like HTTPS, SSH, and SSL/TLS provide encrypted authentication and communication, stopping unauthorized access and eavesdropping — reducing the risk of data breaches and man-in-the-middle attacks.

Enable SSH in OpenWrt via `System > Administration > SSH Access`, select the interface and port, then **Save & Apply**. You can also add specific public SSH keys for passwordless login.

## Manage Traffic Rules

Network devices let you create and implement traffic rules that accept or deny traffic. For example, if you notice data being exfiltrated to a command-and-control server IP, you can create a rule blocking all traffic to that destination IP.

Add/edit traffic rules via `Network > Firewall > Traffic Rules > Add`.

## Monitor Traffic

Keeping track of network traffic — uploads and downloads over time — is essential for a network administrator. For example, unusually large uploads from an email server to an unknown IP address is the kind of alert that enables timely remedial action before significant data loss occurs.

View real-time traffic statistics via `Status > Realtime Graph > Traffic`.

## Configuring Port Forwarding

Port forwarding lets inbound traffic from the internet (or other sources) be routed to a specific device or service on the internal network, while blocking any traffic that doesn't match a defined rule. It's useful for hosting applications that need outside access or remotely controlling internal devices.

!!! warning
    Port forwarding must be configured carefully — it can expose internal devices and services to security issues if misconfigured. Threat actors can also add new port-forward rules to establish connections to external command-and-control servers, so existing rules should be reviewed periodically, not just configured once.

Configure port forwarding via `Network > Firewall > Port Forwards > Add`.

## Monitoring Scheduled Tasks

It's important to monitor scheduled tasks to confirm that the original task list hasn't been modified by a threat actor — cron-based persistence is a common technique once a device is compromised. Add or remove scheduled tasks (handled by cron) via `System > Scheduled Tasks > Save`.

## Update Firmware

Regularly updating the firmware and installed packages helps avoid known and unknown attacks against outdated device software. Update firmware via `System > Software`.

## Additional Techniques for Enterprise Environments

Enterprise network devices generally present an increased attack surface, given the variety of devices, models, makes, and types involved. There's no single set of definite rules, but a few important techniques:

- **Configuring port security** — limit the number of MAC addresses registered on a switch port, and take action whenever unauthorized access is detected. This lets an administrator confirm traffic is coming from a valid source and being forwarded to a legitimate receiver.
- **Preventing ARP spoofing** — ARP spoofing is one of the most common vectors for man-in-the-middle attacks on a network. Mitigate it by enabling static ARP tables and implementing MAC address filtering.
- **Preventing rogue DHCP servers** — an attacker can stand up a spoofed DHCP server to assign IPs to clients and launch MITM attacks. Mitigate with static DHCP binding and network mapping tools to catch unknown devices added to the network.
- **Enabling IPv6** — unlike IPv4, IPv6 has built-in support for IPsec, which secures network communication and provides confidentiality, integrity, and authenticity — helping protect against MITM, eavesdropping, and packet tampering in transit.
