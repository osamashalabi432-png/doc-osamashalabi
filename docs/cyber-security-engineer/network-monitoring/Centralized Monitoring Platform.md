- the idea here is that we are not monitoring our infrastructure, we want one central PRTG system to monitor the infrastructure of multiple customers

```
                  YOUR COMPANY
             ┌─────────────────────┐
             │     PRTG Core       │
             │                     │
             │ Dashboard           │
             │ Alerts              │
             │ Historical data     │
             │ Reports             │
             │ Customer views      │
             └─────────┬───────────┘
                       │
                       │ Encrypted probe connection
                       │
        ┌──────────────┼────────────────┐
        │              │                │
        ▼              ▼                ▼
 CUSTOMER A        CUSTOMER B       CUSTOMER C
 Remote Probe      Remote Probe     Remote Probe
      │                  │               │
      │                  │               │
 ┌────┴────┐        ┌────┴────┐     ┌────┴────┐
 │Firewall │        │Firewall │     │Firewall │
 │Switches │        │Servers  │     │Switches │
 │Servers  │        │VMware   │     │Servers  │
 │VMware   │        │Storage  │     │UPS      │
 │Storage  │        └─────────┘     └─────────┘
 └─────────┘
```

---
#### How we will gather info from the Clients
- by using the Remote Probe, so the probe collects data locally and sends it back to the MSP's central PRTG 

---
### Step by Step to create this CMP

PRTG Core at your company → Remote Probe at customer → monitor one firewall with **Ping + SNMP**.

#### Step 1 — Prepare one Windows machine at the customer
- we will create a small windows VM inside the customer's network for the remote Probe
- aessler currently supports Windows Server 2016/2019/2022/2025 and Windows 10/11, with .NET Framework 4.7.2 or later; Paessler recommends .NET 4.8 for new installations
- 