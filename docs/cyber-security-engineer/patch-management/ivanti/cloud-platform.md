# Ivanti Patch Management, Cloud Platform

## Ivanti Neurons for Patch Management Cloud

it means: getting the client's paid licence turned into a working cloud tenant they can log into.

- Ivanti provisions the tenant (the client's own instance on Ivanti's cloud, e.g. onetec.ivanticloud.com`)
- The subscription entitlement is applied: licence count, which Neurons modules, the term dates
- The first admin account is created and the client takes ownership
- Tenant basics get set: region/data residency, time zone, initial admin users, SSO if in scope
---
#### **CSV Connector**
an inbound connector (Ivanti calls it Generic File Import). It lets you pull device or user data into Neurons from a CSV file so those assets show up in the platform alongside agent-reported data.

- The CSV connector is an **on-premises** connector. It needs a connector server — a machine running the Neurons Agent with a connector-enabled agent policy. That's infrastructure the client must provide.
- It's file-based import, not a live sync. If anyone at SABIL expects automatic inventory pull from Active Directory or SCCM, that is a different SKU and not covered by this line. Better to raise that now than at UAT.