# Microsoft Security Compliance Toolkit

## Installing Security Baselines

Microsoft publishes security baselines in consumable formats, such as Group Policy Object backups, so organizations don't have to derive secure configuration settings from scratch. They're downloaded as zip files and extracted locally.

To download and install the security baselines for Windows Server:

```text
Open Microsoft Security Compliance Website > Download >
Windows Servers Security Baseline.zip > Download
```

Then:

```text
Open extracted folder > Scripts > select desired baseline > execute with PowerShell
```

!!! warning
    Only download security baselines from the official Microsoft website. Because these baselines are applied with elevated Group Policy privileges, a tampered baseline from an unofficial source is a direct path to compromising every machine it's applied to.

## Policy Analyser

The Policy Analyser, another Security Compliance Toolkit feature, compares group policies to quickly surface inconsistencies, redundant settings, and needed changes between them.

Consider an environment where many GPOs are applied at different levels (domain, OU, site) — conflicting or redundant settings accumulate over time, and the Policy Analyser is what makes those conflicts visible and resolvable, rather than relying on manual review of every GPO.

Like the security baselines, the Policy Analyser is downloaded as a zip file from the [same Microsoft download page](https://www.microsoft.com/en-us/download/details.aspx?id=55319). Once extracted, run `PolicyAnalyzer.exe` to add and manage local or domain-level policies.
