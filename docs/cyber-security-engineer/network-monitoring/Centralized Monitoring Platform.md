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

