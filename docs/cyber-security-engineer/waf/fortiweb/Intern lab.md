1. FortiWEB can't be hosted in docker without a license, so it won't work for this lab

2. we will have 3 components here:
    1. FortiWEB VM
    2. Kali linux VM
    3. Target VM


3. here is the final layout

| Machine         | Role              | Networks           | IP                     |
| --------------- | ----------------- | ------------------ | ---------------------- |
| **Kali**        | Attacker          | Host-only #1 + NAT | 10.10.1.5              |
| **FortiWeb-VM** | WAF               | Host-only #1 + #2  | 10.10.1.10 / 10.10.3.1 |
| **Target VM**   | Juice Shop + DVWA | Host-only #2 only  | 10.10.3.2              |
| Intern's laptop | Browser / GUI     | Host-only #1       | 10.10.1.1              |

### Target machine information:

1. `docker-compose.yml` - juice-shop published on host port 3000 - dvwa published on host port 80 - Pinned image tags, not `latest` — I need reproducible builds - `restart: unless-stopped` on both - Minimal, no reverse proxy, no extra services

2. `netplan/01-lab.yaml` - Static 10.10.3.2/24, gateway 10.10.3.1 - No DNS servers needed (offline), but don't break if none are set - Written for Ubuntu Server 24.04 LTS 

3. `setup.sh` - Runs ONCE on a fresh Ubuntu Server 24.04 install, as root, WITH internet - Installs Docker Engine + compose plugin from the official repo - Applies the netplan config - Pre-pulls both images so the VM works offline afterwards - Installs the compose file to /opt/lab and enables it at boot - Fully non-interactive, idempotent, `set -euo pipefail` - Prints a clear success summary at the end 

4. `verify.sh` - Confirms both containers are up and both ports answer HTTP - Confirms the static IP is actually 10.10.3.2 - Exits non-zero with a readable message if anything is wrong 

5. `BUILD.md` — my supervisor build runbook. Must cover: - VMware VM settings (1–2 vCPU, 2 GB RAM, 20 GB disk, single NIC on host-only #2) - Ubuntu Server install choices (no GUI, OpenSSH yes, no snaps) - Running setup.sh and verify.sh - **Cleanup before export**: truncate machine-id, remove SSH host keys, clear logs and bash history, `cloud-init clean` if present, zero free space so the OVA compresses small - Exporting to OVA from VMware - How to sanity-check the exported OVA by re-importing it once 

6. `README-intern.md` - Five lines maximum. Import OVA, attach to host-only #2, power on, don't log in. - Include the note that DVWA's "Create / Reset Database" is a deliberate first-time step inside the lab guide, not a fault. 

7. `kali-hosts.txt` - The /etc/hosts lines interns add on their Kali box so Lab 3 works: js.cloudteamapp.com and student.fwebtraincse.com both pointing at 10.10.1.10 - With a one-line comment explaining why (the ML model was trained against those hostnames, so the Host header must match)