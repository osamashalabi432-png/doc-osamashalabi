# Cloud Security Concepts

Provider-agnostic cloud security concepts that apply regardless of whether the workload runs on Azure, AWS, or elsewhere: how data needs to be protected at each stage of its life in the cloud, and how the risk profile changes depending on which deployment model (private, public, community) is in use.

## Security aspects in the cloud data lifecycle

Data moving through the cloud goes through distinct phases, and each phase has its own security considerations — protecting data "in general" isn't enough, because the risks at creation time are different from the risks at rest, in use, or being destroyed.

### Create / Update

The initial phase: newly created data, or data freshly imported from other sources. This is where the data owner should be defined and the data classified or categorized. Security considerations:

- **Implementing SSL/TLS** — secure communication should be implemented so an attacker can't listen in on data moving between the customer and the cloud provider.
- **Encryption** — data should be encrypted so that if it's exposed, an attacker can't read it without decrypting it.
- **Secure connections** — secure paths should be established for data transfer to minimize the chance of a data breach (data security in transit).

### Store

Data is processed based on its form (structured or unstructured) and stored in a container — generally a database. Security considerations:

- **Encryption** — data should be encrypted to protect it at rest.
- **Backup** — backups should be taken to prevent data loss; if data is lost, it can be restored from an available backup.

### Use

Encrypted data has to be decrypted before an application can use it, which reintroduces risk at this stage. Security considerations:

- **Secure connections** — encrypted paths should be established before data transfer, to protect confidentiality and integrity of data in transit.
- **Secure platform** — a secure authentication mechanism should be used, one that's protected from attacks and vulnerabilities.
- **Restrict permissions** — data owners should set strict permissions so unauthorized users can't modify or process the data.
- **Secure virtualization** — cloud computing shares resources between customers through virtualization, so the provider needs to ensure one customer's data is never visible to another customer.

### Share

Sharing data within or outside the cloud infrastructure introduces its own challenges:

- **Jurisdiction** — regulatory mandates or restrictions on sharing data across specific locations or regions.
- **Data Loss Prevention (DLP)** — DLP helps detect and prevent data breaches or unwanted destruction of sensitive data, keeping it from being shared with unauthorized persons.

### Archive

Long-term storage of data and applications. Security considerations:

- **Encryption** — data should be encrypted before it's stored on cloud premises.
- **Physical security** — storage servers need to be physically secured against unauthorized access (biometrics, CCTV, etc.).
- **Location** — where data physically lives matters: environmental factors (natural disasters, climate) pose risks, and jurisdictional aspects (local and national laws) are a key factor at this stage.
- **Backup procedure** — how data will be recovered when required, and how often full/incremental backups are carried out.

### Destroy

Data should be destroyed once it's no longer needed, so it can't be misused (intentionally or unintentionally). **Crypto shredding** is one way to do this: the encrypted data itself is left in place, but the cryptographic keys are destroyed — without the keys, the data can never be decrypted, effectively rendering it useless.

## Cloud security risks by deployment model

!!! note "Deployment-model risk overview"
    The source notes for this section only contain a section header with no content underneath — general risk categories by deployment model aren't documented here yet. The risks that *are* documented are broken out by deployment model below, under access management.

## Security through access management

How access is managed — and what can go wrong — differs depending on which cloud deployment model is in use.

### Private Cloud

A private cloud is an environment where resources are dedicated to a single customer. It's suited to customers who are more concerned about the security of their data, but it isn't risk-free:

- **Personnel threats** — both unintentional and intentional. The customer has no control over the provider's data center or its administrators, and any insider can cause damage to the customer's data.
- **Natural disasters** — a private cloud is still vulnerable to natural disasters affecting the physical facility.
- **External attacks** — unauthorized access, man-in-the-middle attacks, and Distributed Denial of Service attacks can all compromise the user's data.

### Public Cloud

In a public cloud, resources are shared among users via virtualization technology. Risks include:

- **Vendor lock-in** — the customer becomes dependent on the provider; it becomes nearly impossible to move data out of the cloud before the end of the contract term, effectively making the customer hostage to the provider.
- **Threat of new entrants** — the same cloud provider may also serve a competitor.
- **Escalation of privilege** — users may attempt to acquire unauthorized permissions; a user who gains illicit administrative access could gain control of devices that process other customers' data.

### Community Cloud

Computing and storage infrastructure is shared between members of a specific community or organization. Risks include:

- **Vulnerability** — any node in a community cloud may have vulnerabilities that also expose other nodes; configuration management and consistent security baselines are difficult (often near-impossible) to enforce across the community.
- **Policy and administration** — enforcing decisions and procedures across a community cloud is challenging, which is itself a security threat.

!!! important
    Choice of deployment model is a security decision, not just a cost or convenience one — private cloud trades cost for isolation, public cloud trades isolation for scale and cost efficiency, and community cloud sits in between while inheriting the coordination problems of a shared, multi-tenant governance structure.
