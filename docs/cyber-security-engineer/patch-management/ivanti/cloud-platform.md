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

- The CSV connector is an **on-premises** connector. It needs a connector server, a machine running the Neurons Agent with a connector-enabled agent policy. That's infrastructure the client must provide.
- It's file-based import, not a live sync. If anyone at SABIL expects automatic inventory pull from Active Directory or SCCM, that is a different SKU and not covered by this line. Better to raise that now than at UAT.
---
#### Ivanti Neurons Workspace

**Ivanti Neurons Workspace is a tool for the help desk.**

Imagine a SABIL employee calls IT and says "my computer is slow."

**Without Workspace:** the help desk guy knows nothing about that computer. He asks questions. He can't see the problem. So he passes the ticket to a senior engineer. The employee waits two days.

**With Workspace:** the help desk guy types the employee's name and immediately sees their computer on his screen — how much disk space is left, what programs are installed, whether it's missing patches, whether it keeps crashing. He sees the problem himself and fixes it in five minutes, from his own desk.

That's the whole idea: **let the junior guy solve problems he used to pass to the senior guy.**

**A few extra things it does:**

- He can take remote control of the computer to fix it directly
- He can click pre-made buttons to do common fixes (restart something, clear something, run a repair)
- It gives each computer a "health score" so IT can see bad machines before anyone complains