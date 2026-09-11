# One Identity PAM — Session Management

Instead of an admin logging straight into a server with a privileged password, Safeguard can broker the session itself: the user requests an RDP or SSH session through Safeguard, and Safeguard launches it on their behalf. The admin password is never typed by the user directly — it stays inside Safeguard.

Two session types were tested hands-on, and in both cases the request came back **auto-approved** rather than waiting on a human approver:

## UC-3: RDP session request

Requesting an RDP privileged session through Safeguard, instead of directly logging in with the admin password. The request was approved immediately — no manual approval step in the way it was configured.

## UC-4: SSH session request

Same pattern over SSH: requesting a privileged SSH session through Safeguard rather than using the admin credentials directly. As with the RDP case, the request was auto-approved.

!!! note
    Auto-approval here is a property of the policy attached to that account/asset, not a universal Safeguard behavior — compare with the approval-required and emergency-access flows in [Policies](policies.md), which are for password checkout rather than session requests.

!!! tip
    Step-by-step screens for both flows are in [Labs](labs.md).
