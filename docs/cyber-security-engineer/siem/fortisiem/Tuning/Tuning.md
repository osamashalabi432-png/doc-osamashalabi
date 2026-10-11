## Step 1: Give FortiSIEM context (CMDB)

Many built-in rules check **CMDB groups**, not single IPs. If the groups are empty or wrong, rules fire on the wrong things or never fire.

### 1.1 Set your home country

Many rules and reports use **My Home** to decide what is "foreign". By default it is **United States of America**.

- Go to **RESOURCES > Country Groups > My Home**.
- Change it to the company's country (and any country where you have offices).
![](../../../../attachments/Pasted%20image%2020261011163341.png)

- click edit then this page will appear:
![](../../../../attachments/Pasted%20image%2020261011163448.png)


### 1.3 Create business services

A **business service** groups the devices behind one business function (for example "Email" or "Payroll"). Incidents get tagged with the business service they hit, so analysts see business impact at once.

- Create them under **CMDB > Business Services**.
- See [Creating Business Services](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/386311/creating-business-services).

### 1.4 Set locations for private IPs

FortiSIEM knows where public IPs are. It does not know your internal sites.

1. Go to **ADMIN > Settings > Discovery > Location**.
2. Click **New**, enter the **IP/IP Range** and its **Location**.
3. Click **Save**, then **Apply**.

### 1.5 Load your users

Incidents are more useful when they show a real person, not just an account name.

1. Add an LDAP credential in **ADMIN > Setup > Credentials** (Access Protocol **LDAP**, Used For **Microsoft Active Directory**).
2. Test it and run discovery on the domain controller.
3. Check the users in **CMDB > Users**.

Source: [Advanced Operations: CMDB](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/47862/cmdb), [Incidents and Cases](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/878341/incidents-and-cases)

## Step 2: Set up watch lists

A **watch list** is a list of "suspicious things" (IPs, users, hosts) that FortiSIEM keeps up to date by itself. When a rule fires, it can add the attacker IP or user to a watch list. Other rules can then use that list.

**Why it matters:** one weak signal alone is noise. The same IP showing up on a watch list _and_ hitting a server is a real incident.

### 2.1 Review the built-in watch lists

- Go to **RESOURCES > Watch Lists**.
- FortiSIEM already has watch lists filled by system rules. Check they are filling up after a few days.

### 2.2 Create your own watch list

1. Go to **RESOURCES > Watch Lists** and click **New**.
2. Give it a name and pick the **Type** (for example IP or string).
3. Set how long an entry stays. If the rule does not fire again for that value in that time, the entry is removed.

**Good first watch lists:**

- Users with repeated failed logins
- Hosts that hit a malware IP or domain
- IPs that scanned your network

### 2.3 Link a watch list to a rule

1. Open the rule in **RESOURCES > Rules** and click **Edit**.
2. Go to **Step 3: Define Action** and click the edit icon next to **Watch List**.
3. Pick the **Incident Attribute** to store (for example Source IP).
4. Move the watch list from **Available** to **Selected** and save.

**Watch out:** the watch list **Type** must match the attribute type. An IP watch list cannot take a string attribute.

### 2.4 Use the watch list in other rules

In a rule condition, filter on an attribute **IN** the watch list. Example: "Source IP IN _Scanner IPs_ watch list AND destination is a critical server".

Source: [Watch Lists](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/297178/watch-lists), [Creating Rules](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/903877/creating-rules)

## Step 3: Turn on the right rules

FortiSIEM ships with **over 500 system rules** in **RESOURCES > Rules**. Do not switch them all on. Turn on only the rules that match the logs you actually collect.

### 3.1 Match rules to your log sources

Use the search box in **RESOURCES > Rules** with these keywords.

|   |   |   |
|---|---|---|
|Log source you have|Search keywords|What you catch|
|Active Directory / Windows|login, brute force, account locked, privileged, group change|Password attacks, admin abuse|
|FortiGate / firewall|scan, denied, malware, botnet, IPS|Scans, C2 traffic, exploits|
|VPN|VPN, impossible travel, foreign|Stolen accounts, odd logins|
|EDR / antivirus|malware, virus, ransomware|Infected hosts|
|Proxy / DNS|malware domain, DGA, suspicious URL|Phishing, C2 domains|
|Servers (Linux)|sudo, root, SSH|Privilege abuse|

### 3.2 Activate rules

**One rule:**

1. Go to **RESOURCES > Rules**.
2. Find the rule.
3. Tick **Active** in the Active column.

**Many rules at once:**

1. Go to **RESOURCES > Rules**.
2. Click the Edit icon and select **Multiple Rules**.
3. Pick the rules in the middle panel and click **>**.
4. In **Select Actions**, set the status and (if you want) the **Severity**.
5. Click **Save**.

### 3.3 Use an approval step (optional)

If more than one person manages rules, require approval before a rule is switched on or off.

- Create roles in **ADMIN > Settings > Role > Role Management**.
- The approver role needs **Approver > Rule Activation/Deactivation** ticked.
- Requests then appear in **TASKS** for approval.

### 3.4 Start small

- **Week 1:** turn on rules for your top 3 log sources only.
- **Week 2:** tune those (Step 4), then add the next group.
- Never add a new batch while the last batch is still noisy.

Source: [Rules](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/796401/rules), [Activating and Deactivating a Rule](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/628614/activating-and-deactivating-a-rule)

## Step 4: Tune out the noise

This is the most important step. Fortinet gives **four ways** to stop a noisy rule. Pick the right one for the problem.

|   |   |   |
|---|---|---|
|Problem|Best fix|Where|
|Rule fires for a whole type of device that is normal (for example DNS servers)|**Modify the rule**|INCIDENTS > List > Actions > Edit Rule|
|Rule fires for one known, safe value (one IP, one user)|**Rule exception**|INCIDENTS > List > Actions > Edit Rule Exception|
|The event itself is useless for detection|**Event dropping rule**|INCIDENTS > List > Actions > Create Event Dropping Rule|
|Performance rule fires too early (CPU, memory, disk)|**Adjust the threshold**|ADMIN > Device Support > Custom Property|

### 4.1 Modify the rule (preferred)

Fortinet notes that changing the rule is often better than adding exceptions. Example: add "Source IP NOT CONTAIN DNS Server" to the condition.

1. Go to **INCIDENTS > List**.
2. Select the noisy incident.
3. Click **Actions > Edit Rule**.
4. Change the condition.

**Important:** you cannot save over a **system rule**. Save it as a **user rule**. Then deactivate the old system rule and activate your new one.

### 4.2 Add a rule exception

Use this for a single known value, or for a time window.

1. Go to **INCIDENTS > List**.
2. Select the incident.
3. Click **Actions > Edit Rule Exception**.
4. Pick an **Attribute**, **Operator** and **Value**.
5. (Optional) Click **+** to make it active only at certain times.
6. Add a **Note** saying why. Save.

**Keep exceptions few.** Each one is a blind spot. Prefer group-based conditions (Step 1) over single IPs.

### 4.3 Create an event dropping rule

Use this when an event type is pure noise. You get two choices:

- **Store the event but do not use it in rules**, or
- **Drop the event completely.**

Steps: **INCIDENTS > List** > select incident > **Actions > Create Event Dropping Rule**. Filter by Reporting, Source or Destination IP, or a regex on the content.

**Tip:** choose "store but do not trigger" if you may need the logs for investigation or compliance.

### 4.4 Adjust thresholds

Some rules use global thresholds. Example: **High Process CPU: Server** uses **Process CPU Util Critical Threshold**.

- **For all devices:** **ADMIN > Device Support > Custom Property** > select > **Edit** > new value > **Save**.
- **For one device:** **CMDB** > select device > **Edit** > **Properties** tab > new value > **Save**.

### 4.5 Understand rule conditions before you edit

A rule condition has three parts:

- **Filter:** which events to look at (for example Event Type = Login Failure).
- **Group By:** how to group them (for example by Source IP and User).
- **Aggregate:** when to fire (for example COUNT(Matched events) >= 5).

**Fortinet examples:**

|   |   |   |
|---|---|---|
|Goal|Group By|Aggregate|
|10 or more events|none|COUNT(Matched events) >= 10|
|One source hits 100+ destinations (scan)|Source IP|COUNT(DISTINCT Destination IP) >= 100|
|One workstation logs in to 5+ accounts on one server|Source IP, Destination IP|COUNT(DISTINCT user) >= 5|

You can chain two patterns with **AND**, **OR**, **FOLLOWED-BY**, **AND-NOT** or **NOT-FOLLOWED-BY**. Example: "5 login failures **NOT-FOLLOWED-BY** a success" tells you the attack did not work.

### 4.6 Set clear conditions

Without a clear condition, incidents stay **Active** forever and the list fills up.

1. Edit the rule and go to **Step 3: Define Action**.
2. Next to **Clear Condition**, click **Edit**.
3. Set a **Time Period** (for example 10 minutes).
4. Pick **the Original Rule Does Not Trigger**, or **the following conditions are met**.

**Note:** for **Scheduled** rules, time is the only clear option.

### 4.7 Give incidents useful titles

In **Step 3: Define Action**, build the **Incident Title** from attributes. Example: `$user` _created_ `$fileName` _on_ `$hostName`. Analysts can then read the list without opening each incident.

To show the column: **INCIDENTS > List by Time > Actions > Change Display Columns > Incident Title**.

### 4.8 Keep a change log

Write down every rule change: date, rule, what changed, why, who. Review it every month and remove changes that no longer make sense.

Source: [Incidents and Cases](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/878341/incidents-and-cases), [Creating Rules](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/903877/creating-rules)

## Step 5: Add threat intelligence

Threat intel feeds give FortiSIEM lists of known-bad IPs, domains, URLs and file hashes (IOCs). Rules then fire the moment one of these shows up in your logs.

FortiSIEM downloads feeds on a schedule you set. It supports CSV feeds and STIX/TAXII feeds, plus built-in sources like FortiGuard.

### 5.1 Turn on FortiGuard IOCs

This needs a FortiGuard IOC subscription on your licence.

1. Go to **RESOURCES > Malware Domains** and select the **FortiGuard Malware Domain** folder.
2. Select the entry, click **More > Update**.
3. Select **Enable IOC Service** and set a schedule.
4. If the Supervisor goes to the internet through a proxy, select **Use Proxy**.
5. Repeat for **RESOURCES > Malware IPs** and **RESOURCES > Malware URLs**.

### 5.2 Add other feeds (optional)

FortiSIEM has guides for these feeds:

- AlienVault OTX
- MISP
- OpenCTI
- Malware Patrol
- Any custom feed over HTTPS

### 5.3 Check the feeds are updating

A feed that stopped updating is worse than none, because you think you are protected.

- Open the feed folder in **RESOURCES** and check the last update time.
- If it is old, click **Update** to force a refresh.
- Search the system logs for "Malware data updated" to confirm.

### 5.4 Make sure the matching rules are active

In **RESOURCES > Rules**, search for "malware" and "IOC". Activate the rules that match traffic or DNS against these lists.

Source: [Working with FortiGuard IOCs](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/574343/working-with-fortiguard-iocs), [Threat Intelligence](https://docs.fortinet.com/document/fortisiem/6.4.2/external-systems-configuration-guide/82472), [Fortinet Community: outdated FortiGuard IOCs](https://community.fortinet.com/fortisiem-34/troubleshooting-tip-resolving-outdated-fortiguard-malware-iocs-domains-ips-and-urls-204284)

## Step 6: Add anomaly detection (machine learning)

Normal rules catch what you already know is bad. **Anomaly detection** learns what is normal for your company, then raises an alert when something is very different.

Do this **after** Steps 1 to 5 are stable. Machine learning on noisy data gives noisy results.

### 6.1 How it works in FortiSIEM

- FortiSIEM has had Machine Learning since **release 7.0.0**.
- The data comes from **FortiSIEM reports**. A report feeds the training, then the live checks.
- Fortinet ships **job templates** in **RESOURCES > Machine Learning Jobs**. They are templates only. You must train them before they run.
- When a job finds an anomaly, the system rule **Machine Learning Anomaly Detected** turns it into an incident.

### 6.2 Set up a job

1. Pick a template in **RESOURCES > Machine Learning Jobs** (for example unusual logins per user).
2. Go to **ANALYTICS > Machine Learning** and import the job.
3. **Prepare:** load the data from the job's report.
4. **Train:** run the training and review the result.
5. Save it. It becomes a **user job**.
6. Set the **Inference** and **Re-training** schedules by editing the job.

### 6.3 Make sure the incident rule is active

In **RESOURCES > Rules**, search for **Machine Learning Anomaly Detected** and check it is **Active**.

### 6.4 UEBA (if you use the FortiSIEM Windows Agent)

If the Windows Agent runs with **UEBA turned on**, FortiSIEM has extra **UEBA AI** rules. They flag unusual user or machine behaviour. Search for "UEBA" in **RESOURCES > Rules** and activate the ones you need.

**Tip:** let a model learn for at least a few weeks of normal activity before you trust its alerts.

Source: [Machine Learning (7.0 User Guide)](https://docs.fortinet.com/document/fortisiem/7.0.3/user-guide/545524), [Machine Learning (7.4 User Guide)](https://docs.fortinet.com/document/fortisiem/7.4.2/user-guide/467606), [Rule: Machine Learning Anomaly Detected](https://filestore.fortinet.com/docs.fortinet.com/fsiem/Public_Resource_Access/7_4_0/rules/PH_RULE_ML_ANOMALY.htm), [UEBA Information](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/73233/ueba-information)

## Step 7: Decide who gets told, and how

An incident nobody sees is useless. But too many emails get ignored. Send alerts only for what needs a human now.

### 7.1 Set up email first

1. Go to **ADMIN > Settings > Email > Email Settings** and set the mail server.
2. (Optional) Make your own template in **ADMIN > Settings > Email > Incident Email Template**. Add the rule's **Remediation Note** so the reader knows what to do.

### 7.2 Create automation policies

In 7.4 this is called **Automation Policy**. Older releases call it **Notification Policy**.

1. Go to **ADMIN > Settings > General > Automation Policy**.
2. Click **New**.
3. Set the filter carefully: **Severity**, **Rules**, **Time Range**, **Affected Items**, **Affected Organizations**.
4. Under **Action**, pick **Send Email/SMS to Target Users**.
5. Enter email addresses, or pick users from CMDB.
6. Click **Save**.

**Fortinet warns:** set the filter carefully to avoid too many emails.

### 7.3 A simple starting plan

|   |   |   |
|---|---|---|
|Severity|Who|How|
|High|On-call analyst + SOC lead|Email/SMS right away, plus auto-create a case|
|Medium|SOC team|Email, reviewed during shift|
|Low|Nobody|Reviewed in the incident list or a daily report|

Adjust after 2 weeks based on what people actually act on.

### 7.4 Control repeat alerts

Each rule has a **Notification** frequency in **Step 3: Define Action**. Use it so one long attack does not send hundreds of emails.

### 7.5 Other actions an automation policy can take

- **Auto-create a case** for the incident.
- **Run a remediation** script (for example block an IP). First check the script exists in **ADMIN > Settings > General > Automation Policy**.
- Send to a ticketing or SOAR tool such as FortiSOAR.

**Start with alerting only.** Add automatic remediation later, once you trust the rule.

### 7.6 Check it works

On any incident: **INCIDENTS > List > Actions > Show Notification History**.

Source: [Incidents and Cases](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/878341/incidents-and-cases), [Creating Rules](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/903877/creating-rules)

## Step 8: Set the analyst workflow

Every incident should end in one of two places: a **case** (real threat) or a **tuning change** (false positive). Nothing should sit open with no owner.

### 8.1 Know the incident views

Go to **INCIDENTS**. There are four views:

- **Overview:** top incident types and hosts at a glance.
- **List:** search and act on incidents. This is where analysts work.
- **Risk:** devices and users ranked by risk from their incidents.
- **MITRE ATT&CK:** incidents grouped by attack tactic.

### 8.2 The triage loop

For each new incident in **INCIDENTS > List**:

1. **Open it.** Read the title, source, target and trigger events.
2. **Decide:** real threat, or false positive?
3. **If real:** create a case, add a comment, notify or remediate.
4. **If false positive:** fix the rule using Step 4, then clear the incident.
5. **Always:** leave a comment so the next person knows what you did.

[embed: node/f6725222-3469]

A false positive is not finished until the rule is fixed. Otherwise the same incident comes back tomorrow.

### 8.3 Useful actions (all under Actions in the List view)

|   |   |
|---|---|
|Action|Use it to|
|Edit Comment|Record what you found|
|Clear Incident|Close it, with a **Reason**|
|Notify via Email|Send it to someone now|
|Remediate Incident|Run a remediation script on a device|
|Edit Rule / Edit Rule Exception|Tune a false positive|
|Create Event Dropping Rule|Stop a useless event type|
|Show Notification History|Check who was told|

### 8.4 Use cases for real threats

A **case** groups one or more incidents into a single investigation with an owner.

- Create one by hand: see [Creating a Case Manually](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/129762/creating-a-case-manually).
- Or create cases automatically with an automation policy: see [Creating a Case Automatically](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/790063/creating-a-case-automatically).

### 8.5 Give analysts the right access

- Create roles in **ADMIN > Settings > Role > Role Management**.
- Assign them in **CMDB > Users > Edit > System Admin**.
- Analysts usually need to read and act on incidents, but not change rules.

Source: [Working with Incidents](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/431486/working-with-incidents), [Incidents and Cases](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/878341/incidents-and-cases), [CMDB](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/47862/cmdb)

## Step 9: Silence planned maintenance

Patching and reboots cause "device down" and "service stopped" incidents. Tell FortiSIEM about maintenance in advance so these do not flood the list.

### 9.1 Create a maintenance calendar

1. Go to **ADMIN > Setup > Maintenance**.
2. Click **New** and give it a name.
3. Set the **Time Range**: start time and duration.
4. Set the **Recurrence Pattern**: one time, weekly on set days, or monthly.
5. Pick the devices or a whole device group. Use **>>** to add a folder.
6. Save.

**Optional:** tick **Generate Incidents for Devices under Maintenance** if you still want incidents during the window.

### 9.2 Calendar or rule exception?

|   |   |
|---|---|
|Situation|Use|
|Regular patch night for a group of servers|Maintenance calendar|
|One rule should ignore one device at certain hours|Rule exception with a time period (Step 4.2)|

### 9.3 Good habits

- Link the calendar to your change process. No approved change, no maintenance window.
- Keep windows short. A long window hides real attacks too.
- Review the calendar every month and delete old entries.

Source: [Configuring Maintenance Calendars](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/172393/configuring-maintenance-calendars), [Creating Rules: Defining Rule Exceptions](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/903877/creating-rules)

## Step 10: Test before you trust

A rule you never tested is a rule you hope works. Test every new or changed rule before you switch it on.

### 10.1 Test a rule with fake events

1. Go to **RESOURCES > Rules** and make sure the rule is **inactive**. If you cannot deactivate it, **Clone** it and test the inactive copy.
2. Select the rule and click **Test**.
3. Enter a **Reporting IP** the fake event comes from. If the rule needs that IP to be in a group (for example network devices), use an IP in that group.
4. Under **Raw Event**, paste a real-looking log line that should trigger the rule.
5. Under **Pause**, set the seconds between events. Click **+** to add more events until the threshold is met.
6. Click **Test Rule**. If it passes, activate the rule.

**Note:** **Scheduled** and **Scheduled via SQL** rules cannot be tested this way.

### 10.2 If the test fails

|   |   |   |
|---|---|---|
|Error|What it means|Fix|
|Rule syntax error|The rule itself is badly written|Check the condition and attributes|
|Conditions not met|The events did not match|Check filter, Group By and count|
|Event parsing error|The log text did not match the parser|Use the exact wording your device sends (for example "denied" vs "deny")|

### 10.3 Test end to end

Fake events prove the rule logic. Also test the full chain once with real activity:

- Do 6 failed logins with a test account. Check the incident appears.
- Check the email or SMS arrived (Step 7).
- Check the case was created, if you set that up.
- Check the incident clears by itself (Step 4.6).
- Write the result in your change log.

Agree any test with the security team first, so nobody treats it as a real attack.

Source: [Testing a Rule](https://docs.fortinet.com/document/fortisiem/7.4.1/user-guide/528244/testing-a-rule), [Testing a Rule (7.0)](https://docs.fortinet.com/document/fortisiem/7.0.0/user-guide/939531)