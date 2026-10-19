### collector problem:
- check the collector
```bash
phstatus
```

- ping between the collector and the supervisor:
```bash
ping <SUPERVISOR-IP>
```

- check the storage
```bash
df -h
```

- If the network and disk are fine, renew the registration with the `--update` option.
```bash
/opt/phoenix/bin/phProvisionCollector --update admin 'YOUR_PASSWORD' <SUPERVISOR-ip> Super forti-coll-01
```

- check the logs:
```bash
tail -n 50 /opt/phoenix/log/phoenix.log
```

---
### Check health inside FortiSIEM GUI

Check the system health page: ***ADMIN > Health > Cloud health***

make sure these are correct:
1. Health status to be normal
2. CPU, memory and disk should not be close to full
3. EPS (Event per second) should be above zero

---
### Check collector

Go to ***ADMIN > Health > Collector Health***

For each Collector, check:
- **Status** is **Up**
- **Last Status Update** time is recent (a few minutes ago)
- **EPS** is above zero

---
### Confirm logs are actually arriving

- Go to **ANALYTICS** and run a search for the **last 10–15 minutes** with no filter.