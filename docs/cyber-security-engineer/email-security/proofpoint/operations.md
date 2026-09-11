# Proofpoint — Logs, Syslog & Bulk Mail

Two smaller operational areas of the Proofpoint console, captured briefly in the source notes.

## Logs and Reports: Syslog Configuration

**Where:** `System` → `Logs and Reports` → `Log Settings`

- **Log file level** — controls how much detail is captured. Setting this to **Informational** collects a large amount of data, which can fill up available disk space if left unmanaged.
- **Retain log files for** — the number of days log files are kept on the system before rotation/deletion.

!!! warning "Source is thin here"
    The notes stop at these two settings. Syslog forwarding to an external destination (e.g. a SIEM) was not covered in the captured content — if that's part of your deployment, it isn't documented yet.

## Bulk Mail Overview

Bulk mail rules govern how Proofpoint identifies and handles bulk/mass-sent mail (such as marketing email) separately from ordinary business correspondence.

!!! note "Near-empty in the source"
    The only note captured is that this section exists "to understand how to enable and edit the bulk email rule." No steps for actually enabling or editing the rule were recorded — this page is intentionally left thin rather than guessing at the procedure.
