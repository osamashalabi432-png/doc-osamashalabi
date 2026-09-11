# FortiWeb — Overview

FortiWeb is Fortinet's Web Application Firewall (WAF) — it provides specialized, application-layer threat detection and protection for HTTP/HTTPS traffic, sitting in front of web applications rather than inspecting network traffic generically the way FortiGate does.

!!! important
    FortiWeb is not designed to protect against non-HTTP attacks. It should always be deployed **behind a firewall** (such as FortiGate), which handles broader network-layer threats while FortiWeb focuses on the web application layer.

## What FortiWeb protects against

FortiWeb's protection is broad, spanning several layers of an HTTP application's attack surface:

- IP reputation
- DDoS protection
- Protocol validation
- Attack signatures
- Antivirus / DLP
- Application-layer inspection
- Third-party integration
- Advanced/behavioral protection (machine learning based)

Broadly, FortiWeb's feature set falls into three categories of WAF functionality:

- **Bot Mitigation**
- **API Protection**
- **Web Security**

## What the engine actually does

FortiWeb's application-aware firewalling and load balancing engine:

- Secures HTTP applications that are often gateways into valuable databases
- Prevents and reverses defacement of websites
- Improves application stability
- Monitors servers for downtime and connection load
- Prevents unknown and zero-day attacks using machine learning

Working together with FortiGate, FortiWeb can also:

- Maximize website performance while maintaining security
- Reduce response times
- Accelerate SSL/TLS, either with dedicated ASIC chips or in software
- Accelerate websites through compression
- Perform real-time content rewriting
- Apply customizable redirection rules to incoming web traffic

## Attack protection methods

At a high level, FortiWeb mitigates attacks using a combination of methods rather than any single technique:

- Signatures
- Anomaly analysis
- IP reputation
- Access control
- Rate limiting
- Authentication
- Page order and other stateful application analysis
- Input sanitization
- Anti-defacement

## Basic administration

FortiWeb ships with a single built-in administrator account, `admin`, which uses the `prof_admin` access profile.

When creating or managing administrator accounts, you can restrict logins to a list of **trusted hosts** — specific IP addresses or subnets allowed to authenticate as that admin.

- If trusted hosts are defined (for example `192.168.1.0/24`), only devices in that range can log in as the admin.
- If the IPv4 trusted host is left at `0.0.0.0/0` ("allow any IP"), anyone who can reach the login page can attempt to log in — including brute-forcing the admin password.

!!! warning
    Leaving trusted hosts at `0.0.0.0/0` makes FortiWeb's admin login vulnerable to brute-forcing from any address that can reach it. Restrict trusted hosts to known management networks.

## Certification blueprint (FortiWeb 7.4)

For reference, the topics below reflect the scope of the Fortinet FortiWeb certification exam at the time these notes were written:

| Field | Value |
|---|---|
| Time allowed | 65 minutes |
| Exam questions | 35–40 questions |
| Scoring | Pass or fail. A score report is available from your Pearson VUE account. |
| Language | English |
| Product version | FortiWeb 7.4 |

Topic areas:

- **Deployment and configuration** — deployment requirements, system settings, server pools/policies/protected host names, high availability (HA), deployment and system troubleshooting
- **Encryption, authentication, and compliance** — mitigating web application vulnerabilities, access control and tracking methods, mitigating attacks on authentication, SSL inspection and offloading, encryption/authentication troubleshooting
- **Web application security** — threat mitigation features, blocking known attacks, threat detection troubleshooting, API protection
- **Machine learning (ML)** — anomaly detection, bot detection, API anomaly detection
