# One Identity PAM — Labs

Hands-on walkthroughs of four Safeguard PAM use cases. Screenshots from the original notes are referenced but not reproduced here.

## UC-1: Request access to privileged credentials — with approval

A support engineer needs to fix a problem on a company server. The server has a powerful account called `srvacc`. Instead of giving the engineer the password permanently, the engineer goes to Safeguard and requests access.

1. Engineer clicks **New Request**.
2. Engineer chooses the server/account.
3. Engineer explains why they need access.
4. Approver reviews the request.
5. If approved, the engineer gets temporary access.
6. Safeguard records who used it, when, and why.

*(Screenshots of the request form in the original notes.)*

Once approved, the password is retrieved through Safeguard's self-service portal (`ssp.iam.corp`) rather than being handed over directly:

- Navigate to `ssp.iam.corp`.
- *(Screenshots of the self-service portal login/flow.)*
- The password is revealed there.

*(Screenshot of the released password.)*

## UC-2: Request access to privileged credentials — emergency

A critical production server goes down after hours, and the normal approver is unavailable. The on-call administrator needs immediate access to the privileged account `srvacc` to restore the service. Instead of waiting for approval or using a shared password, the administrator uses **Emergency Access** in Safeguard, enters a reason such as "production outage," gets temporary access, fixes the issue, and the activity is logged for later review.

*(Steps captured as a sequence of screenshots in the original notes — the emergency-access request form, justification entry, and the resulting temporary access grant.)*

## UC-3: RDP session request via Safeguard — auto-approved

Requesting an RDP privileged session through Safeguard, instead of directly logging in with the admin password.

*(Screenshots of the session request and launch.)*

The request was approved immediately — auto-approval was in effect for this account/policy combination.

## UC-4: SSH session request via Safeguard — auto-approved

Requesting an SSH privileged session through Safeguard, instead of directly logging in with the admin password.

*(Screenshots of the session request and launch.)*

As with UC-3, the request came back auto-approved.

!!! note
    See [Policies](policies.md) for the approval-workflow concepts behind UC-1/UC-2, and [Session Management](session-management.md) for the concepts behind UC-3/UC-4.
