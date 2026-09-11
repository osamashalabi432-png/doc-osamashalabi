# FortiSIEM Hardening

!!! note
    This is a hardening checklist for FortiSIEM Supervisor/Worker nodes. For deployment, configuration, and operations documentation, see the [FortiSIEM product pages](../../siem/fortisiem/index.md) under SIEM.

Baseline OS-level and network-level hardening for FortiSIEM nodes, based on Fortinet's official hardening guidance.

| Topic | Description |
|---|---|
| [OS Security](os-security.md) | Public CA-signed SSL certificates and disk encryption for `/cmdb`, `/svn`, and `/data`. |
| [Network Security](network-security.md) | Restricting network access, standard management ports, moving HTTPS/SSH to non-standard ports, and disabling unused interfaces. |
