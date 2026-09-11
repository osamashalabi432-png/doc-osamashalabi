# FORTRA — Overview

FORTRA's relevant content here covers two adjacent ideas: classifying data so it can actually be protected, and Tripwire, a file-integrity-monitoring product that overlaps with DLP concerns around unauthorized change.

## Data Classification

**Data classification** is the process of consistently categorizing data — using visual and metadata labels — based on pre-defined criteria.

Classification enables you to:

- Avoid taking a "one size fits all" approach to data protection, which is inefficient.
- Avoid arbitrarily choosing what data to spend protection resources on, which is risky.

!!! note
    In other words: you can't prioritize DLP controls sensibly on data you haven't first classified — classification is what tells you where the sensitive data actually is.

## Tripwire

Tripwire is a **file-integrity-monitoring (FIM)** product. It's included here because unauthorized changes to configuration files are a data-protection concern adjacent to DLP, even though Tripwire itself is not a DLP product.

- **Scope:** Tripwire only monitors **system configuration files** — it does not monitor arbitrary documents such as Word or PDF files.
- **Change control:** Tripwire has a built-in ticketing system, used to authorize modifications an employee makes to a monitored file.
- **Agent requirement:** a lightweight agent must be installed on every machine to be monitored.
- **Pricing/training notes:** training costs **$1,200**. A free trial is available but requires signing an NDA.

!!! warning "Thin source"
    The captured notes on Tripwire end here — deployment steps, policy configuration, and alerting were not recorded.
