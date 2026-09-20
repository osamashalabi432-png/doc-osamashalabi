# FortiGate — Overview

FortiGate is Fortinet's next-generation firewall (NGFW) platform, used to enforce perimeter and internal network security — inspecting and controlling traffic between network segments rather than just permitting or denying it at the IP/port level.

## Initial Access

Before a FortiGate appliance can be configured, you need to reach its web-based GUI (or CLI) for the first time. Out of the box it has no network presence on your existing LAN, so getting to it requires a direct, local connection.

### Connecting to the device

Every FortiGate has a dedicated **MGMT** port for out-of-band management. Connect this port directly to your laptop (or into the same local segment) to reach the administration interface.

!!! note
    If the specific model doesn't have a dedicated MGMT port, any of the numbered interface ports can be used instead to reach the GUI.

For CLI-only management (useful for initial bootstrapping or when the GUI isn't reachable), FortiGate also has a **console port**, which connects via a serial/console cable rather than Ethernet.

### Logging in for the first time

Once connected over Ethernet, browse to the default management URL:

```text
https://192.168.1.99
```

!!! tip
    If the page doesn't load, your laptop's IP is probably outside FortiGate's default subnet. Change your NIC's IP to something else in the `192.168.1.0/24` range (for example `192.168.1.100`) so it can reach `192.168.1.99`.

The default administrator credentials are:

| Field | Value |
|---|---|
| Username | `admin` |
| Password | *(none — blank)* |

!!! warning
    Change the default password immediately after first login. An admin account with no password on a reachable management interface is a straightforward compromise path.

## Lab guide

Hands-on FortiGate firewall lab guide (PDF, 5.8 MB):

[:material-file-pdf-box: Download the FortiGate Firewall Lab Guide](FortiGate%20Firewall%20Lab%20Guide%20_%20PDF%20_%20Command%20Line%20Interface%20_%20Transport%20Layer%20Security.pdf){ .md-button download }
