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

---
### Is there any logs arriving? 
```bash 
tcpdump -i any -nn udp port 514
```

- can the collector reach the supervisor in azure: ***ADMIN → Settings → System → Cluster Config***

---
# How to make an administrator changes appear in FortiSIEM
#### Step 1: Get a Meraki API key

In Meraki Dashboard:

1. **Organization → Settings** → turn on **Dashboard API access**.
2. Click your **profile (top right) → My profile → API access** → **Generate new API key**.
3. Copy it and keep it safe. Don’t share it in chat.

#### Step 2: Test it from the Collector

```bash
curl -H "Authorization: Bearer <API-KEY>" \
  https://api.meraki.com/api/v1/organizations
```

Note the **id** in the result. That’s your Organization ID.

Then test the change log:

```bash
curl -H "Authorization: Bearer <API-KEY>" \
  https://api.meraki.com/api/v1/organizations/<ORG-ID>/configurationChanges
```


#### Step 3: Set it up in FortiSIEM

Go to **ADMIN → Setup → Credentials → New**

- **Device Type:** Generic → **HTTPS Advanced** (Generic Log API Poller)
- **URL:** the configurationChanges URL from Step 2
- **Header:** `Authorization: Bearer <API-KEY>`
- **Pull interval:** 5 minutes


### ✏️ 1. General Parameters

- **Host / Server:** `api.meraki.com`
- **Port:** `443`
- **Protocol:** HTTPS

---

### ✏️ 2. Authentication Parameters

Meraki accepts the key in a simple header:

- **Header name:** `X-Cisco-Meraki-API-Key`
- **Value:** your API key

(If the form asks for a “prefix” or “Bearer” style instead, use header `Authorization` with value `Bearer <API-KEY>`.)

---

### ✏️ 3. Log API Parameters

- **Method:** GET
- **Endpoint / Path:**  
    `/api/v1/organizations/<ORG-ID>/configurationChanges`
- **Query parameter:** `timespan` = `600`  
    (means “changes in the last 10 minutes”)
- **Events location in response:** the root of the JSON (it’s a list of changes)