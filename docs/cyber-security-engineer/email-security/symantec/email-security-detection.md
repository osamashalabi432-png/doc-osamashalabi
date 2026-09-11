# Symantec — Email Security & Detection (ETDR)

**Email Threat Detection and Response (ETDR)** is Symantec's layer for catching what signature-based filtering misses — zero-day threats and targeted attacks — by combining several detection technologies rather than relying on any single engine.

## Detection technologies

ETDR employs three detection technologies:

- **Skeptic** — a cloud-based service that detects new, emerging malware and variations of older malware. It uses heuristic scanning and machine-learning methods to block previously-unseen threats, including zero-day threats.

- **Cynic** — in addition to blocking known threats, ETDR sends copies of files of interest to the Symantec **Cynic** cloud sandbox service. Cynic:
    - Launches suspicious files in a secure sandbox and mimics typical end-user behavior across various operating system environments, to trigger any malicious behavior.
    - Moves execution from a virtual to a **physical** environment when necessary, to catch malware that detects and evades virtual machines ("VM-aware" malware).
    - Correlates results with the Symantec Global Intelligence Network to determine whether a file is malicious.
    - Keeps monitoring files that stay inactive in the sandbox, watching for later attempts to move laterally or communicate with a command-and-control server.
    - Can be enabled or disabled, with a configurable maximum hold time while analysis runs — if Cynic doesn't return a verdict within that time, the message is delivered anyway. If later analysis finds malware, Cynic can alert an administrator.

- **Synapse** — a Symantec cloud service that correlates events recorded across Symantec Endpoint Protection, Symantec Endpoint Detection and Response, and Symantec Email Security.cloud.
    - Your Symantec.cloud portal credentials must be entered into the Endpoint Detection and Response administration console before Synapse will include Email Security.cloud incidents in its correlations — this explicitly authorizes Email Security.cloud to export data to Synapse.
    - Correlated data lands in the Symantec Endpoint Detection and Response Manager, **not** the Symantec.cloud management portal — that's where you view results, run reports, or export to a syslog destination such as a SIEM.

## Configuring Cynic settings

**Where:** `Services` → `Email Threat Detection and Response` → `Cynic Settings`

1. On the **Cynic Settings** tab, check **Enable** to send attachments to Cynic, or uncheck it to disable Cynic analysis.
2. Choose a **Maximum Hold Time** from the dropdown.
    - Selecting `0` makes Cynic do an instantaneous scan for known malicious attachments only, and the message is delivered immediately.
3. Click **Save**.

## Domain-level settings

Each ETDR settings screen lists your organization's domains:

- The default selection is **Global Settings**, which applies changes to every domain unless a specific domain already has custom settings.
- To customize a single domain, select it from the Global Settings list.

!!! warning
    Once a domain has custom settings applied, you cannot change it back to inheriting the global settings.

## Defining sender and administrator email addresses

**Where:** `Services` → `Anti-Malware`

1. On the **Alert Settings** tab, under **Email Addresses**, either accept the default sender address `alerts@notifications.messagelabs.com` or enter a different **Sender Email** address.
2. Under **Administrator Email**, enter an Anti-Malware administrator's email address and click **Add**.
3. Repeat for each additional administrator who should be notified.
4. Click **Save** at the bottom of the tab.
