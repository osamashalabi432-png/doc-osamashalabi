# Implementing the Least Privilege Model

## Creating the Right Type of Accounts

Implementing least privilege starts with using distinct account types for distinct purposes, rather than one account doing everything:

- **User accounts** — regular accounts for most people in the network, used for the routine duties of their role. This should be the default for most users.
- **Privilege accounts** — accounts with elevated privileges, further classified as first- and second-level privilege accounts. These should be separate from a person's day-to-day user account.
- **Shared accounts** — accounts shared among a group of people (e.g. visitors, with bare-minimum privileges for a limited time). These are not recommended and should only be used in limited scenarios, since shared credentials make it impossible to attribute actions to a specific individual.

## Tiered Access Model (TAM)

The Active Directory Tiered Access Model is a set of technical controls that reduce the risk of privilege escalation by creating logical boundaries around AD's assets, based on how valuable/sensitive they are.

!!! important "Why tiering matters"
    Without tiering, a compromised low-value asset (like a single end-user's laptop) can be a stepping stone to compromising Domain Controllers, because credentials and administrative sessions cross freely between tiers. Tiering enforces that a credential used to manage Tier 0 assets is never exposed on a Tier 1 or Tier 2 machine — which is exactly the pattern attackers exploit to escalate from a phished workstation to full domain compromise.

The primary goal is protecting Active Directory's top-valued identities (Tier 0), while still letting domain members and other users perform routine tasks — email, browsing, apps — at Tier 1/2 without those activities putting Tier 0 at risk.

The model has three tiers:

- **Tier 0** — the top level: all admin accounts, Domain Controllers, and the groups that control them.
- **Tier 1** — domain member applications and servers.
- **Tier 2** — end-user devices (e.g. HR and sales staff — non-IT personnel).

The rule of thumb: credentials and administrative sessions from a higher tier should never be used on, or exposed to, a lower tier.
