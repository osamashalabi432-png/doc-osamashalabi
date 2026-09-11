# Network Security

## Restrict Network Access

FortiSIEM only needs certain ports open — for communicating with external systems and with its own cluster members. See [Appendix A of Fortinet's hardening guide](https://docs.fortinet.com/document/fortisiem/7.4.1/hardening-guide/582961/hardening-fortisiem-security#Appendix) for the full list of ports open by default.

You may want to close ports not applicable to your environment — for example, SNMP Trap on port 162 if you're not using SNMP traps. Use `firewalld` and `firewall-cmd` to block unneeded ports.

It's recommended to place the Supervisor and Worker nodes in a separate, isolated network segment, protected by a firewall that restricts access to only the ports and protocols needed for management and monitoring — minimizing the attack surface of the FortiSIEM cluster itself.

## Typical Management Ports

The following ports are essential for administrative access to FortiSIEM nodes:

| Port | Purpose |
|---|---|
| HTTPS (TCP/443) | Secure web-based access to the FortiSIEM GUI |
| SSH (TCP/22) | Command-line management and troubleshooting of Supervisor and Worker nodes |

For other ports to consider opening, see [Appendix A](https://docs.fortinet.com/document/fortisiem/7.4.1/hardening-guide/582961/hardening-fortisiem-security#Appendix).

## Change HTTPS and SSH Ports to Non-Standard Ports

Moving management services off their default, well-known ports reduces exposure to automated scanning and opportunistic attacks that target the default ports.

!!! warning
    Currently, changing the SSL default port will fail and is not recommended at this time.

### Changing the HTTPS Port

By default, HTTPS is on port 443. To change it:

1. SSH to the FortiSIEM node.
2. Open `/etc/httpd/conf.d/ssl.conf` for writing.
3. Change `Listen 443` to `Listen <your port>`.
4. Change `<VirtualHost _default_:443>` to use `<your port>`.
5. Restart the service: `systemctl reload httpd`.
6. Check status: `systemctl status httpd`.

For each port change, open the new port in the FortiSIEM firewall so the inbound connection is still allowed:

```text
# firewall-cmd --add-port <your port>/tcp --permanent
# firewall-cmd --reload
```

### Changing the SSH Port

By default, SSH is on port 22. To change it:

1. SSH to the FortiSIEM node.
2. Open `/etc/ssh/sshd_config` for writing.
3. Change `Port 22` to `Port <your port>`.
4. Restart the service: `systemctl reload sshd`.
5. Check status: `systemctl status sshd`.

For each port change, open the new port in the FortiSIEM firewall:

```text
# firewall-cmd --add-port <your port>/tcp --permanent
# firewall-cmd --reload
```

## Disable Unused Interfaces

In VM deployments, only one network interface is used. On hardware appliances, an interface can become live simply by plugging a cable into it — so unused interfaces should be administratively disabled rather than left live and unmonitored.

To disable an interface:

1. SSH to the FortiSIEM node.
2. Run:

```bash
sudo ifconfig <your interface> down
```
