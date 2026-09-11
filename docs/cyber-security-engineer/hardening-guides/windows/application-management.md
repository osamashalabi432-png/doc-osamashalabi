# Application Management

## Trusted Application Store

The Microsoft Store offers a curated range of applications (games, utilities) and allows downloading vetted files through a single click. Malicious actors commonly bind legitimate software with trojans and viruses and upload it elsewhere on the internet to infect and gain access to a victim's computer — downloading from the Microsoft Store instead reduces the chance of running trojanized software.

Access the Microsoft Store by typing `ms-windows-store:` in the Run dialog.

## Safe App Installation

You can restrict the machine to only allow installation of applications from the Microsoft Store, closing off the most common software-based infection vector (downloading trojanized installers from arbitrary websites).

Go to `Settings > Apps and Features` and select **The Microsoft Store only**.

## Malware Removal Through Windows Defender Antivirus

Windows Defender used to have its own dedicated interface; Windows 10 and newer manage it through the Windows Security Center instead. Windows Defender primarily offers four functionalities:

- **Real-time protection** — periodic scanning of the computer.
- **Browser integration** — scans all downloaded files during safe browsing.
- **Application Guard** — complete web-session sandboxing to block malicious websites or sessions from making changes to the computer.
- **Controlled Folder Access** — protects memory areas and folders from unwanted applications.

## AppLocker

AppLocker allows blocking specific executables, scripts, and installers from running, through a set of rules — configurable on a single PC or across a network via GUI. This lets you move from a "blocklist" mindset (block known-bad) to an "allowlist" mindset (only known-good runs), which is far more effective against unknown or novel malware.

## Protecting the Browser Through Microsoft SmartScreen

Microsoft SmartScreen helps protect against phishing/malware sites and software when using Microsoft Edge. It:

- Displays an alert when visiting suspicious web pages.
- Vets downloads by checking their hash/signature against a malicious software database.
- Protects against phishing and malicious sites by checking visited websites against a threat intelligence database.

Turn it on via `Settings > Windows Security > App and Browser Control > Reputation-based Protection`, then enable the **SmartScreen** option.

!!! tip
    In Microsoft Edge, also go to `Settings > Privacy, Search and Services` and set **Tracking prevention** to **Strict** to reduce tracking through ads, cookies, etc.
