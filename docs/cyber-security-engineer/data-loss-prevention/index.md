# Data Loss Prevention

Controls focused on stopping sensitive data from leaving the organization — whether through screen capture, uncontrolled file/config changes, or a lack of classification to begin with.

| Section | Description |
|---|---|
| [Data Patrol](data-patrol/index.md) | Screen Watermark — a visible, traceable watermark for insider-threat deterrence. |
| [FORTRA](fortra/index.md) | Data Classification concepts, plus Tripwire for file-integrity monitoring. |

!!! note
    This category is intentionally thin — it reflects what was captured in the source notes for each product, not a complete DLP program.

## Symantec DLP

Symantec's primary product line in these notes is [Email Security](../email-security/symantec/index.md), but it also has a DLP offering. The only captured note on it concerns upgrading the DLP agent:

- The registry entry for the **TDI drivers** has to be deleted before upgrading the DLP agents.

!!! warning "Very thin source"
    This is the entirety of what was captured on Symantec DLP — no further configuration, policy, or deployment detail is available.
