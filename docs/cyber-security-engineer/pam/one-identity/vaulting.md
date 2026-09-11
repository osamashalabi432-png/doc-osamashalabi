# One Identity PAM — Vaulting

## Safeguard for Privileged Passwords (SPP)

SPP is the credential-vaulting component of One Identity's Safeguard suite. It stores privileged account passwords centrally and issues them to a requester only after a checkout request goes through (approved or auto-approved, depending on policy) — rather than distributing them permanently or letting them sit in a shared document. Every checkout is logged (who, when, why).

This is what the UC-1 and UC-2 walkthroughs in [Labs](labs.md) exercise end-to-end: an engineer requests the password for `srvacc`, and once the request clears, retrieves it through Safeguard's self-service portal instead of it being handed out directly.

!!! note "Reading plan tracked, not yet written up"
    The source notes for SPP itself are a reading outline (a list of page ranges from the official manual to work through — entities/overview, first-time setup, account automation, security policy management, disaster recovery, troubleshooting, ports) rather than first-hand configuration notes. See [References](references.md) for that list. Once that reading is done, this page should be expanded with the actual vault architecture, credential onboarding, and rotation configuration.
