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

- Check the system health page: ***ADMIN > Health > Cloud health***
