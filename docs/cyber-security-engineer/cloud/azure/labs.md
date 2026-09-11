# Microsoft Azure — Labs

Hands-on labs that build up a small piece of Azure infrastructure incrementally: an SSH key pair, a VM, a couple of Virtual Networks, a subnet, a public IP, and then wiring a managed disk, a NIC, and a public IP onto existing VMs. Each lab was done through the Azure Portal.

!!! note
    Lab numbering follows the original source material, which jumps from Lab 2 to Lab 4 — Lab 3 doesn't exist in the source notes, so it's skipped here rather than invented.

## Lab 1 — SSH key pair

**Goal:** Create an SSH key pair named `datacenter-kp`, of type `rsa`, for authenticating into VMs.

1. In the Azure Portal, search for **SSH key** and open the SSH keys service, then select **Create**.
2. Choose the resource group, set the key pair name to `datacenter-kp`, and set the key pair type to **RSA**.
3. Review the settings to confirm they validate.
4. Create the key pair — Azure generates the key and it appears in the SSH keys list.

!!! tip
    An SSH key pair is created up front so it's ready to attach when a VM is provisioned — Azure VMs are commonly set up for key-based SSH login instead of a password.

## Lab 2 — Create a virtual machine

**Goal:** Create an Azure VM named `devops-vm` and confirm SSH access to it.

Requirements:

- Use the existing resource group.
- VM name `devops-vm`, region **West US**.
- Image: **Ubuntu 22.04 LTS**.
- Size: **Standard_B1s**.
- A default Network Security Group (NSG) allowing inbound SSH (port 22).
- A 30 GB **Standard HDD** data disk.
- All other settings left at their defaults.

Steps:

1. Search for **Virtual machines** in the Azure Portal and select **Create**.
2. Select the resource group, name the VM `devops-vm`, set the region to **West US**, choose the **Ubuntu 22.04 LTS** image, and set the size to **Standard_B1s**.
3. Configure the disk: 30 GB, **Standard HDD**.
4. Under networking, allow inbound port **22 (SSH)** so the VM gets a default NSG rule permitting SSH.
5. Review and validate the configuration.
6. Create the VM, then confirm it can be reached — connect to it over SSH from a Windows machine.

## Lab 4 — Create a VNet

**Goal:** Create a Virtual Network named `datacenter-vnet` in **East US**, with any IPv4 CIDR block.

1. Search for **Virtual network** in the Azure Portal and select **Create**.
2. Choose the resource group, name the VNet `datacenter-vnet`, and set the region to **East US**.
3. Accept the default IPv4 address space, since this lab doesn't require a specific CIDR block.
4. Validate the configuration and create the VNet.

See [Virtual Networks](networking/virtual-networks.md) for why this step matters — a VNet is the private network foundation everything else (VMs, subnets, NSGs) gets placed into.

## Lab 5 — Create a VNet with a specific IPv4 CIDR

**Goal:** Create a Virtual Network named `datacenter-vnet` in **East US**, this time with a specific `192.168.0.0/24` IPv4 CIDR block, since later labs provision different services under different VNets.

1. Search for **Virtual network** in the Azure Portal and select **Create**.
2. Choose the resource group, name the VNet `datacenter-vnet`, and set the region to **East US**.
3. Set the IPv4 address space explicitly to `192.168.0.0/24`.
4. Validate the configuration and create the VNet.

## Lab 6 — Create a subnet in a VNet

**Goal:** Create a Virtual Network named `xfusion-vnet` in **East US** with an IPv4 address range of `10.0.0.0/16`, and one subnet named `xfusion-subnet` inside it.

1. Search for **Virtual network** in the Azure Portal and select **Create**.
2. Choose the resource group, name the VNet `xfusion-vnet`, set the region to **East US**, and set the address space to `10.0.0.0/16`.
3. On the IP addresses step, add a subnet named `xfusion-subnet` within that address space.
4. Deploy — the VNet and its subnet deploy successfully.

## Lab 7 — Create a public IP address

**Goal:** Allocate a Public IP address named `devops-pip`.

1. Search for **Public IP addresses** in the Azure Portal and select **Create**.
2. Choose the resource group and region, and set the name to `devops-pip`.
3. Create the resource.

!!! note
    A public IP address is a standalone resource in Azure — it isn't attached to a VM automatically. See Lab 10 for attaching one to a VM's network interface.

## Lab 8 — Attach a managed disk to a VM

**Goal:** Attach an existing managed disk, `xfusion-disk`, to an existing VM, `xfusion-vm` (East US), as a data disk.

1. Search for **Virtual machines**, and open the existing VM `xfusion-vm`.
2. Go to the VM's **Disks** section.
3. Use the option to attach an existing disk, and select `xfusion-disk`.
4. Confirm the disk shows as attached to `xfusion-vm`.

!!! warning
    Make sure the VM has finished initializing before attaching the disk — the task required VM initialization to be complete before this step is submitted.

## Lab 9 — Attach a NIC to a VM

**Goal:** Attach an existing network interface (NIC), `datacenter-nic`, to an existing VM, `datacenter-vm` (West US).

1. Search for **Virtual machines**, and open the existing VM `datacenter-vm`.
2. Attach the existing NIC `datacenter-nic` to the VM.
3. Confirm the NIC's status shows as **attached** before considering the task complete.

## Lab 10 — Attach a public IP to a VM

**Goal:** Attach an existing public IP, `xfusion-pip`, to the network interface of an existing VM, `xfusion-vm-pip`.

1. Locate the existing VM `xfusion-vm-pip` and its network interface.
2. Attach the public IP `xfusion-pip` to that network interface's IP configuration.
3. Confirm the VM is properly assigned the public IP.

!!! tip
    This is the same pattern as Lab 7 + Lab 9 combined: the public IP and the NIC are both created/attached as separate resources, then joined together on the VM's network interface.
