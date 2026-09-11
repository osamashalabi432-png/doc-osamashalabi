# One Identity PAM — Policies

Access to privileged credentials in Safeguard is governed by a request policy attached to the account. Two patterns were tested hands-on:

## Approval-based access (UC-1)

**Scenario:** a support engineer needs to fix a problem on a server that has a powerful account (`srvacc`). Instead of that engineer holding the password permanently, they request access through Safeguard and an approver has to sign off before it's released.

The flow:

1. Engineer clicks **New Request**.
2. Engineer chooses the server/account.
3. Engineer explains why they need access.
4. Approver reviews the request.
5. If approved, the engineer gets temporary access.
6. Safeguard records who used it, when, and why.

This is the default, safest pattern — it trades a bit of latency (waiting on an approver) for a human check on every privileged access grant.

## Emergency access (UC-2)

**Scenario:** a critical production server goes down after hours and the normal approver is unavailable. Waiting for approval isn't an option, and falling back to a shared password would break the audit trail. Safeguard's **Emergency Access** path lets the on-call administrator get immediate access to `srvacc` by entering a justification (e.g. "production outage"), without waiting for an approver — the access is still time-limited and the activity is still logged for later review.

This exists specifically so "no approver is available" never becomes a reason to bypass Safeguard entirely — the break-glass path keeps the request inside the audited system instead of outside it.

!!! tip
    Full click-by-click walkthroughs of both flows (with the actual screens used) are in [Labs](labs.md).
