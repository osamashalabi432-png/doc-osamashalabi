# Palo Alto — WildFire

WildFire is Palo Alto's cloud-based malware analysis and prevention service. When a file or URL looks suspicious or unknown, the firewall sends it to the WildFire cloud for analysis rather than making a local, isolated decision about it.

> WildFire is like a cloud malware analysis lab that helps the firewall decide whether a file is safe.

## Verdicts

WildFire evaluates a file or URL and returns one of four verdicts:

| Verdict | Meaning |
|---|---|
| **Benign** | Safe. |
| **Grayware** | Suspicious/unwanted, but not confirmed malware. |
| **Malware** | Malicious and harmful. |
| **Phishing** | A fraudulent link trying to trick users. |

## How the flow works

**Firewall receives file → checks it → asks WildFire if needed → gets a verdict → allows or blocks it**

## Email inspection

When an email passes through the network, the Palo Alto firewall can inspect both the **attachment** and the **URL inside the email**. If something looks suspicious, it's sent to WildFire for analysis, which returns a verdict — and that verdict can update protection for every other user on the platform, not just the one who triggered it.

## How WildFire differs from traditional sandboxing

WildFire goes beyond a typical standalone sandbox in three main ways:

| Method | Simple meaning |
|---|---|
| **Global community data** | Learns from threats seen across many environments, not just your own. |
| **Advanced analysis techniques** | Uses more than one analysis method to catch modern, evasive attacks. |
| **Automated prevention** | Quickly turns a detection into protection across the entire environment. |

## Summary

WildFire does three main things:

1. Receives suspicious files or links.
2. Analyzes them and produces a verdict.
3. Creates protection and shares it automatically.
