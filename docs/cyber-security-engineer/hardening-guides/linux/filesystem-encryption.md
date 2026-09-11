# File-System Partitioning & Encryption

Encrypting the disk means that if the physical drive (or a copy of it) is stolen, or an attacker mounts it on another system, the data on it is unreadable without the decryption key/passphrase — physical access to the hardware no longer implies access to the data.

There are various encryption tools available for Linux, but the default and most widely used is **LUKS** (Linux Unified Key Setup). LUKS handles the on-disk format for encrypted volumes and integrates with the standard Linux disk-encryption tooling (`cryptsetup`), so encrypted partitions can be set up consistently across distributions.

!!! tip "Best practice"
    Enable disk encryption at install time when possible — partitioning and encrypting an already-populated production disk after the fact is far riskier and more disruptive than setting it up during initial provisioning.
