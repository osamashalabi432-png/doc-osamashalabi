# One Identity PAM — Overview

One Identity's privileged access management product is called **Safeguard**. It is made up of two components:

- **Safeguard for Privileged Passwords (SPP)** — the credential-vaulting piece. It stores privileged account passwords and hands out access only after a request is made (and, depending on policy, approved), instead of giving people permanent or shared passwords. See [Vaulting](vaulting.md).
- **Safeguard for Privileged Sessions (SPS)** — the sessions component of the same suite, for RDP/SSH privileged sessions launched through Safeguard.

!!! note
    The source notes for SPS are an empty outline — no first-hand content has been captured for it yet, so that side of the suite isn't documented here.

## Why it's used

Instead of an engineer holding a powerful account's password (e.g. `srvacc`) indefinitely, they request access to it through Safeguard when they actually need it. Depending on the policy attached to that account, the request either goes to an approver or is auto-approved, and every checkout/session is logged — who used it, when, and why.

Two request patterns were exercised hands-on and are documented in this section:

- **Requesting credentials** (approval-based, and a break-glass emergency path) — see [Policies](policies.md).
- **Requesting a session** (RDP/SSH, launched directly through Safeguard rather than typing the admin password in) — see [Session Management](session-management.md).

Full step-by-step walkthroughs of both, with the actual screens used, are in [Labs](labs.md).
