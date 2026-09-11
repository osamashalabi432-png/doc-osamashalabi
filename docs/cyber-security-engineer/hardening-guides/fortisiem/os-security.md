# OS Security

## Enable Public CA-Signed SSL Certificates

In FortiSIEM 7.x and 6.x, all external communication is via SSL, unless collecting data from a device forces the use of another protocol (for example, NetFlow or SNMP). Protecting that SSL communication with certificates signed by a public Certificate Authority — rather than self-signed certificates — avoids the trust and validation weaknesses that come with self-signed certs (clients silently trusting an unverified certificate, or users being trained to click through certificate warnings).

For configuration details, see [Configuring CA Certificates](https://docs.fortinet.com/document/fortisiem/7.4.1/configuring-ca-certificates).

## Enable Disk Encryption

Encrypting disks prevents a malicious actor from removing a drive and reading its contents on an external system.

!!! warning
    FortiSIEM does not recommend encrypting the root disk — doing so introduces an operational challenge, since it requires supplying a passphrase during every boot-up.

Instead, encrypt the three additional data disks:

| Disk | Contents |
|---|---|
| `/cmdb` | Postgres database |
| `/svn` | Device configuration and monitored files |
| `/data` | Logs |

Steps for encrypting any of these disks are covered in Fortinet's [disk encryption documentation](https://docs.fortinet.com/document/fortisiem/7.4.1/disk-encryption-of-data-on-the-supervisor).
