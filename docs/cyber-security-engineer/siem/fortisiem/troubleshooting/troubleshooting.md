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

```
curl -sI https://hole.cert.pl/domains/v2/domains.csv
```

| List                  | Pick                            | Why                                                                               |
| --------------------- | ------------------------------- | --------------------------------------------------------------------------------- |
| **Malware Domains**   | **Tweetfeed Malware Domain**    | Free and global                                                                   |
| **Malware IPs**       | **Emerging Threat Malware IP**  | Free, low false alarms, matches firewall logs                                     |
| **Malware Hash**      | **Malware Bazaar Malware Hash** | Free, large, daily                                                                |
| **Malware URLs**      | **URLHaus Malware URL** ✅       | Free (abuse.ch, same people as Malware Bazaar), focused on malware download links |
| **Malware Processes** | **None for now**                | See below                                                                         |

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
---
### 1.1 Create a read-only admin (recommended)

A Meraki API key has the same rights as the admin who owns it. A key from a full admin could change the network if it leaks.

1. Meraki Dashboard > **Organization > Administrators** > **Add admin**.
2. Name: `fortisiem-api`, email: a mailbox you control.
3. Organization access: **Read-only**.
4. Save, then log in once as that admin to accept the invite.

### 1.2 Turn on API access

1. **Organization > Settings**.
2. Find **Dashboard API access** and turn it on.
3. Save.

### 1.3 Generate the API key

1. Log in as the read-only admin.
2. Click the profile icon (top right) > **My profile**.
3. Under **API access**, click **Generate new API key**.
4. Copy the key once and store it in your password vault. Meraki will not show it again.

Never paste the key in chat, tickets or screenshots.

**To confirm:** Meraki moves menus from time to time. If a path above is different, search the dashboard for "API".

## Part 2: Test the API from FortiSIEM

Before touching the GUI, prove that the FortiSIEM node can reach Meraki and read the change log.

Run these on the node that will do the polling (the Collector, or the Supervisor if you poll from it). Log in by SSH as root.

### 2.1 Check internet access to Meraki

```bash
curl -sI https://api.meraki.com
```

- Any HTTP response (200, 301, 404) = the node reaches Meraki.
- Timeout = outbound HTTPS is blocked. Fix the firewall first.

### 2.2 Get the Organization ID

```bash
curl -s -H "X-Cisco-Meraki-API-Key: <API-KEY>" \
  https://api.meraki.com/api/v1/organizations
```

The result lists your organizations. Copy the `id` of the right one.

- `401` = wrong key, or API access is off (Part 1.2).
- `[]` (empty) = the admin has no access to any organization.

### 2.3 Read the change log

Make a small, harmless test change in Meraki first (for example, edit a network's notes). Then run:

```bash
curl -s -H "X-Cisco-Meraki-API-Key: <API-KEY>" \
  "https://api.meraki.com/api/v1/organizations/<ORG-ID>/configurationChanges?timespan=600"
```

You should see your test change, with fields like `ts`, `adminName`, `adminEmail`, `networkName`, `page`, `label`, `oldValue`, `newValue`.

**Save this output.** You need one real record later to test the parser.

If you don't see your change, stop here. FortiSIEM will not see it either.

After testing, clear the key from shell history:

```bash
history -c
```

## Part 3: Create the credential in FortiSIEM

The credential tells FortiSIEM where to call, how to log in, and how to tag the events.

Path: **ADMIN > Setup > Credentials > Step 1: Enter Credentials > + (New)**

### 3.1 Main screen (Verified)

|   |   |
|---|---|
|Field|Value|
|Name|`Meraki Admin Changes`|
|Device Type|Generic|
|Access Protocol|HTTPS Advanced|
|Pull Interval|5 Minute(s)|
|Authentication Type|API Key|
|Description|Pulls Meraki Organization change log via Dashboard API|

Do not pick Access Protocol **HTTPS** or Password config **CyberArk REST API**. Those are for other integrations.

The screen then shows three pencil buttons: General Parameters, Authentication Parameters, Log API Parameters.

### 3.2 General Parameters (Verified)

|   |   |
|---|---|
|Field|Value|
|Host Name|`api.meraki.com`|
|URI Stem|`/api/v1/organizations/<ORG-ID>/configurationChanges`|
|HTTP Method|GET|
|Disable SSL Certificate Check|Unchecked|
|JSON Response Log Key|Empty|
|Log Header|`[Meraki_Admin_Change]`|

- **URI Stem** is the path, not the port. There is no port field; HTTPS uses 443.
- **JSON Response Log Key** is empty because Meraki returns a plain list with no wrapper key. To confirm: the test in Part 4 shows if FortiSIEM accepts this.
- **Log Header** is a tag added to the start of every event. The parser uses it later to recognise these events.

Click **Save**.

### 3.3 Authentication Parameters (Verified)

|   |   |
|---|---|
|Field|Value|
|API Key Name|`X-Cisco-Meraki-API-Key`|
|API Key Value|the API key from Part 1.3|
|Send Method|Send As Header|

Click **Save**.

### 3.4 Log API Parameters (partly verified)

The screen has four tabs: **Header**, **Parameter**, **Body**, **Pagination**.

- **Header** (Verified): leave empty. The API key is already sent by 3.3.
- **Parameter** (To confirm): click **+** and add key `timespan`, value `600`. This asks Meraki only for the last 10 minutes of changes.
- **Body**: leave empty. A GET request has no body.
- **Pagination** (To confirm): leave empty. Ten minutes of changes fits in one page (Meraki returns up to 5000 records by default).

Why 600 seconds with a 5-minute pull: if one poll is late, the next one still covers the gap. Some changes may be pulled twice. To confirm in testing: a repeat usually updates the same open incident instead of opening a new one.

Click **Save**, then **Save** on the main screen.

## Part 4: Link the credential and test it

The mapping tells FortiSIEM which node runs the credential and against which host.

Path: **ADMIN > Setup > Credentials > Step 2: Enter IP Range to Credential**

### 4.1 Pick the polling node (Verified)

At the top left of Step 2 there is a dropdown that shows **Use Supervisor** by default.

- Pick the **Collector** that tested OK in Part 2, if the events must belong to that site or client.
- Keep **Use Supervisor** only if the Supervisor tested OK in Part 2 and that is your design.

### 4.2 Create the mapping (Verified)

Click **+**. The window **Device Credential Mapping Definition** opens.

|   |   |
|---|---|
|Field|Value|
|IP/Host Name|`api.meraki.com`|
|Credentials|`Meraki Admin Changes`|

The Credentials dropdown may preselect another credential (for example **FGT-REST-API**). Change it before you save.

Click **Save**.

### 4.3 Test

1. Select the new row.
2. Click the **flask icon** (Test Connectivity).
3. Wait for the result.

The flask icon is verified. The result messages below are expected outcomes, to confirm against what FortiSIEM actually shows.

|   |   |   |
|---|---|---|
|Result|Meaning|Fix|
|Success|FortiSIEM reached Meraki and read the change log|Go to Part 5|
|401 / auth error|Key wrong or API access off|Recheck 3.3 and Part 1.2|
|404|Wrong path or ORG-ID|Recheck URI Stem in 3.2|
|Timeout|Node can't reach api.meraki.com|Rerun Part 2.1 on that node|
|JSON / parse error|FortiSIEM wants a JSON Response Log Key|Send the error text for review|

## Part 5: Check that events arrive

Before building a parser or rule, confirm the raw events are reaching FortiSIEM.

1. Make a small, harmless test change in Meraki.
2. Wait 5 to 10 minutes (one or two pulls).
3. Go to **ANALYTICS**.
4. Set the time range to the last 1 hour.
5. Add a filter: **Raw Event Log** CONTAIN `Meraki_Admin_Change`.
6. Run the search.

**What you should see:** one event per change. The event type will be generic or unknown, because no parser exists yet. That is expected.

**Copy one full raw event** (right click > show raw, or the raw event column). Hide anything sensitive. This is the sample for the parser in Part 6.

**No events after 15 minutes?**

- Recheck the test result in Part 4.3.
- Confirm the change shows in Meraki **Organization > Change log**.
- Confirm the polling node from 4.1 is healthy in **ADMIN > Health**.

To confirm: the exact filter name (Raw Event Log) and the raw event view may differ slightly in 7.6.

## Part 6: Event Type, Parser, Rule

All screens in this part are **To confirm**: none has been checked in 7.6.0 yet. Take a screenshot of each screen before saving, and finalise the parser against the real event from Part 5.

### 6.1 Event Type

Path (to confirm): **RESOURCES > Event Types > +**

|   |   |
|---|---|
|Field|Value|
|Name|`Meraki-Admin-Config-Change`|
|Description|Meraki dashboard configuration changed by an admin|
|Device Type|Cisco Meraki (or Generic if not listed)|
|Severity|5 (medium)|
|Category|the configuration change category|

### 6.2 Parser (draft)

Path (to confirm): **ADMIN > Device Support > Parsers > +**

Each Meraki change record carries these fields:

|   |   |
|---|---|
|Meraki field|Meaning|
|`ts`|time of the change|
|`adminName`, `adminEmail`|who made it|
|`networkName`|which network|
|`page`, `label`|what was changed|
|`oldValue`, `newValue`|before and after|

Draft parser. It is not live until enabled. Test it first.

```xml
<eventParser name="MerakiAdminChangeParser">
  <deviceType>
    <Vendor>Cisco</Vendor>
    <Model>Meraki</Model>
    <Version>ANY</Version>
  </deviceType>

  <eventFormatRecognizer><![CDATA[\[Meraki_Admin_Change\]]]></eventFormatRecognizer>

  <parsingInstructions>
    <collectFieldsByRegex src="$_rawmsg">
      <regex><![CDATA[\[Meraki_Admin_Change\]\s*<_json:gPatMesgBody>]]></regex>
    </collectFieldsByRegex>

    <collectAndSetAttrByJSON src="$_json">
      <attrKeyMap attr="user" key="adminName"/>
      <attrKeyMap attr="emailAddr" key="adminEmail"/>
      <attrKeyMap attr="hostName" key="networkName"/>
      <attrKeyMap attr="objName" key="page"/>
      <attrKeyMap attr="objType" key="label"/>
      <attrKeyMap attr="oldValue" key="oldValue"/>
      <attrKeyMap attr="newValue" key="newValue"/>
    </collectAndSetAttrByJSON>

    <setEventAttribute attr="eventType">Meraki-Admin-Config-Change</setEventAttribute>
  </parsingInstructions>
</eventParser>
```

Open points before enabling:

- The real raw event format (from Part 5) decides the regex. Adjust it to match.
- Check that the attributes `emailAddr`, `objName`, `objType`, `oldValue`, `newValue` exist in **ADMIN > Device Support > Event Attributes**. Create any that are missing, or map to existing ones.

Safe test:

1. Paste the XML in the parser editor.
2. In the Test area, paste the real raw event from Part 5.
3. Run the test and check `user`, `oldValue`, `newValue` are filled.
4. Only then: **Save > Enable > Apply**.

### 6.3 Rule

Path (to confirm): **RESOURCES > Rules > +**

|   |   |   |
|---|---|---|
|Section|Setting|Value|
|General|Name|Meraki Admin Configuration Change|
|General|Severity|Medium (High if the client asks)|
|General|Category|Security / Change|
|Condition|Filter|Event Type = `Meraki-Admin-Config-Change`|
|Condition|Aggregate|COUNT(Matched Events) >= 1|
|Condition|Group By|`user`|
|Condition|Time window|300 seconds|
|Action|Incident fields|`user`, `hostName`, `objName`, `oldValue`, `newValue`|

Save, then set the rule to **Active**.