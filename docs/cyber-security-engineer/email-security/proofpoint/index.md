# Proofpoint — Overview

Proofpoint is a cloud email security platform. The deployment covered in these notes is built on the **P0 bundle**, which brings together three components:

- **Email Protection** — the core inbound/outbound email gateway: spam detection, imposter (display-name spoofing) protection, and attachment/URL scanning.
- **Threat Response Auto-Pull (TRAP)** — automatically pulls (removes) malicious messages from user mailboxes after delivery, once a threat is confirmed.
- **Targeted Attack Protection (TAP)** — detects and analyzes advanced threats (malicious URLs and attachments) delivered via email, and reports on them through its own dashboard.

## In this section

| Page | Description |
|---|---|
| [Technical Configuration](configuration.md) | Imposter Email Display Name Repository (SCSS), URL Defense, and Attachment Defense setup. |
| [Targeted Attack Protection Dashboard](dashboard.md) | Navigating the TAP dashboard: filters, reports, search, and tools. |
| [Email Communication Process](email-flow.md) | How a message moves from sender to receiver — MUA → MTA → MDA. |
| [Customizing Your Email Digest](digest.md) | Setting up a digest schedule and a custom SMTP sender profile for it. |
| [Logs, Syslog & Bulk Mail](operations.md) | Brief operational notes on log retention and bulk mail rules. |

!!! note
    These pages reflect what was captured in the source notes at the time of writing. Several sub-areas of the Proofpoint console (attachment defense exceptions, syslog forwarding, bulk mail rule editing) were only briefly touched on in the original notes — those gaps are called out explicitly on the relevant pages rather than filled in with assumptions.
