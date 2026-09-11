# Firewall

A host-based firewall controls which ports and services are reachable on the machine, which directly shapes its attack surface — any port left open unnecessarily is a potential entry point. Before writing new rules, you need to know what's currently exposed.

To see what open ports exist on the firewall (`ufw` — Uncomplicated Firewall):

```bash
sudo ufw status
```

!!! tip
    Review this output as a routine check, not just a one-time setup step — services get installed over time and can silently open new listening ports that were never intended to be exposed.
