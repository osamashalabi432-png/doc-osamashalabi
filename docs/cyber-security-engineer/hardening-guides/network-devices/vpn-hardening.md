# Hardening Virtual Private Networks

## Use a Strong Encryption Algorithm

Configure the VPN gateway to use strong encryption to protect data in transit. In OpenVPN, the `cipher` directive in the config file selects the encryption scheme. Options include AES, Blowfish, Camellia, and more — for example, **AES-128-CBC** means the AES algorithm with a 128-bit key in Cipher Block Chaining (CBC) mode. **AES-256-CBC** is typically considered one of the strongest cipher choices today.

```bash
sudo nano /etc/openvpn/server/server.conf
```

## Keep VPN Gateway Software Up-to-Date

Ensure the VPN gateway software always has the latest security patches — each VPN product has its own update mechanism. For OpenVPN on a Debian-based system:

```bash
sudo apt upgrade openvpn
```

## Implement Strong Authentication

Use strong authentication mechanisms — a combination of Transport Layer Security (TLS) and a secure hashing algorithm. The `auth` directive in the OpenVPN configuration specifies the exact hashing algorithm used for packet authentication. Options include **SHA1, SHA128, SHA256, SHA512, and MD5** (SHA256 or stronger is recommended over legacy options like MD5).

```bash
sudo nano /etc/openvpn/server/server.conf
```

```text
local 10.10.189.208
port 1194
proto udp
dev tun
ca ca.crt
cert server.crt
auth SHA256          # use this to change the auth parameter
tls-crypt tc.key
topology subnet
```

## Change Default Settings

Change default usernames and passwords to something unique, reducing the risk of unauthorized access to the VPN gateway — default credentials on VPN appliances are a well-known and commonly exploited weakness.

## Enable Perfect Forward Secrecy (PFS)

PFS in OpenVPN generates a unique session key for each session, strengthening the connection's security.

!!! important "Why this matters"
    Because each session gets its own fresh encryption keys, even if an attacker successfully obtains one session's key, they cannot use it to decrypt other sessions — past or future. This prevents an attacker who later compromises a long-term key from retroactively decrypting previously captured traffic.

Enable PFS with the `tls-crypt` directive. Generate the required key with:

```bash
sudo openvpn --genkey --secret my.key
```

and place it in the same directory on the server. Combining a strong cipher and auth setting (e.g. `cipher AES-256-CBC` and `auth SHA256`) with `tls-crypt` supports PFS:

```text
local 10.10.189.208
port 1194
proto udp
dev tun
ca ca.crt
cert server.crt
cipher AES-256-CBC
auth SHA256
tls-crypt my.key      # use this to change the tls-crypt parameter
tls-version-min 1.2   # use this to change the minimum TLS version
topology subnet
```

## Dedicated Users for the VPN Server

Limit user access by creating a dedicated user account and group with restricted permissions specifically for running the OpenVPN server — so a compromise of the VPN process doesn't automatically grant broader system access.
