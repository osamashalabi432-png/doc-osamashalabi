# SOC — Alert Management

A SOC's job largely comes down to handling a stream of alerts: deciding which platform an alert lives in, what its properties mean, and when it's serious enough to hand off to someone else. Getting these basics consistent across a team is what makes triage fast and repeatable instead of ad hoc.

## Alert management platforms

Alerts don't all need to be managed in the same type of tool — the right platform depends on team size and how many detection sources need to be aggregated.

| Solution | Examples | Description |
|---|---|---|
| SIEM | Splunk ES, Elastic | SIEMs have solid alert management capabilities and are a perfect choice for most SOC teams. |
| EDR or NDR | Microsoft Defender, CrowdStrike | EDR and NDR provide their own alert dashboards, but it's preferred to route alerts through a SIEM or SOAR instead. |
| SOAR | Splunk SOAR, Cortex SOAR | Bigger SOC teams can use a SOAR to aggregate and centralize alerts from multiple solutions. |
| ITSM | Jira, TheHive | Some teams run a custom ticket-management (ITSM) setup on a dedicated solution. |

## Alert properties

Every alert, regardless of which platform raised it, carries the same core set of properties. Knowing what each one means is what lets an analyst triage an alert quickly.

| # | Property | Description | Examples |
|---|---|---|---|
| 1 | Alert Time | Shows alert creation time. The alert usually triggers a few minutes after the actual event. | Alert Time: March 21, 15:35 · Event Time: March 21, 15:32 |
| 2 | Alert Name | A summary of what happened, based on the detection rule's name. | Unusual Login Location · Email Marked as Phishing · Windows RDP Bruteforce · Potential Data Exfiltration |
| 3 | Alert Severity | Defines the urgency of the alert. Initially set by detection engineers, but can be altered by analysts if needed. | 🟢 Low / Informational · 🟡 Medium / Moderate · 🟠 High / Severe · 🔴 Critical / Urgent |
| 4 | Alert Status | Shows whether someone is currently working the alert or whether triage is done. | 🆕 New / Unassigned · 🔄 In Progress / Pending · ✅ Closed / Resolved · (plus other custom statuses) |
| 5 | Alert Verdict | Also called alert classification — explains whether the alert is a real threat or noise. | 🔴 True Positive / Real Threat · 🟢 False Positive / No Threat · (plus other custom verdicts) |
| 6 | Alert Assignee | The analyst assigned to (or who self-assigned to) review the alert. Sometimes called the alert owner. | The assignee takes responsibility for their alerts. |
| 7 | Alert Description | Explains what the alert is about, usually in three parts. | The logic of the alert-generating rule · why the activity can indicate an attack · optionally, how to triage this alert |
| 8 | Alert Fields | SOC analysts' comments and the specific values that triggered the alert. | Affected Hostname · Entered Commandline · (and many more, depending on the alert) |

## When to escalate

Escalation exists so that alerts requiring authority, expertise, or action beyond a first-line analyst's remit don't stall in a single person's queue. Escalate an alert if:

1. It's an indicator of a major cyberattack requiring deeper investigation or DFIR (Digital Forensics and Incident Response).
2. Remediation actions are required — malware removal, host isolation, or a password reset.
3. Communication with customers, partners, management, or law enforcement is required.
4. You simply don't fully understand the alert and need help from a more senior analyst.

!!! tip
    Escalation criterion 4 is worth normalizing on a team: escalating because you don't understand an alert is not a failure — it's the process working as intended.
