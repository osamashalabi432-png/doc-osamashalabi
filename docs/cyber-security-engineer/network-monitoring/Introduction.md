- PRTG is an infrastructure monitoring platform for availability, performance, and network. the main question that it asks:

```
Are my firewalls, switches, servers, applications, links, and services healthy — and if not, alert me.
```

#### what PRTG contains?
1. Dashboard
2. Database 
3. Alerts
4. Reports
5. Configuration

### Concepts of PRTG
##### 1. Core Server
- this is the brain it handles:
	1. configuration
	2. dashboards
	3. historical monitoring data
	4. reports
	5. user accounts
	6. thresholds
	7. notifications
	8. the web interface

##### 2. Probe
- this is the monitoring engine like it's an actual software that performs monitoring, it communicates with your devices and retrieves information 

- It runs as a service on a machine, receives monitoring instructions from the PRTG core sever

- there is two types of probe:
	1. local probe
	2. remote probe

```
HQ
PRTG Core + Local Probe
        │
        ├── Firewall
        ├── Switches
        └── Servers


Branch Office
Remote Probe
        │
        ├── Firewall
        ├── Switches
        └── Servers
```

##### 3. Sensor
- monitoring element that you configure inside the PRTG
- the sensor tells the probe what to monitor and how to monitor it