
- Agility is about speed to adapt, like provisioning new servers fast. 
- Elasticity is about automatic scaling, resources grow/shrink with demand. 
- Agility is human-driven; elasticity is automated.

#### what is cloud computing?
model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

**abstraction**: involves creating virtual machines (VM) from physical servers
**orchestration** automates and coordinates the provisioning of these VMs and their networking to CSC

---
![](../../../attachments/Pasted%20image%2020260917200809.png)

**top**: What it must do (access, scale, measure, self-serve).  
**Middle**: What kind of service you get (SaaS, PaaS, IaaS).  
**Bottom**: Where it’s hosted (public, private, hybrid, community).

---
#### Essential characteristics: 
**cloud pools**: share resources like CPUs and storage to serve many users at once like a shared resturant kitchen

**Broad Network Access**: Services are available over the network and accessed through web browsers or specialized applications while using heterogeneous thin client platforms 

**Rapid Elasticity:** Cloud resources grow or shrink instantly — like turning on a faucet to get more water or turning it off to save. You don’t wait for hardware; you get what you need, when you need it, automatically.

**Measured Service**: Cloud tracks how much you use — storage, bandwidth, users — and bills you only for what you consume, like a utility meter. You pay for what you actually use, not what you own.

**On-Demand Self-Service**: You can request cloud resources anytime, without waiting for someone to approve or manually set up the hardware — it’s automatic.

---
### Cloud Service Models
IaaS: it gives you virtual machiness, storage and networking, the customer manage the OS and the apps, the cloud provider manages the hardware

PaaS: 

| Feature               | IaaS (Infrastructure as a Service)           | PaaS (Platform as a Service)               |
|-----------------------|---------------------------------------------|-------------------------------------------|
| What you get          | Virtual machines, storage, networking       | OS, runtime, DB, dev tools                |
| What you manage       | OS and applications                         | Only your code                            |
| What provider manages | Hardware (physical servers)                | Infrastructure + OS + runtime             |
| Example               | AWS EC2, Azure VMs                         | Google App Engine, Heroku                |