# Delinea PAM — Policies

## Just-in-Time (JIT) access

Instead of giving a user permanent admin rights, PAM can grant them only for the moment they're needed, then take them away automatically:

- **Temporary, time-limited privileges**
- Only when needed
- Automatically removed afterward

In practice the flow is: you need admin → you request it → you get it for a limited time → time ends → admin goes away. This shrinks the window during which a compromised account has elevated rights, without permanently blocking people from doing their jobs.

## Deny policy

A deny policy blocks a specific action rather than granting one. Example configuration path: **Action → search for "deny" → Deny read/write access to Microsoft Office document files → duplicate** (to base a new rule off the template).

There's also **Deny Quarantine**, configured under **Admin → Filters**, by searching for the specific item to quarantine.

!!! warning
    Deny/quarantine policies can crash the endpoint or block a legitimate service if scoped too broadly — see the best-practice notes below on testing before wide rollout.

## Best practices

- Keep a **test environment** — validate policies there before pushing to production; a bad policy can crash the endpoint or block services it wasn't meant to touch.
- Limit the policies that are allowed to *block* services, for the same reason.

### Naming policy

A consistent naming convention makes it possible to tell what a policy does, and its priority, just from its name:

```text
<Action>-<Target>-<Filter>
```

Example: `Block-Sales-Visualstudio`

Numeric prefixes are also used to signal priority/specificity — e.g. a rule named `123` is a high-priority, very specific **Allow** rule for a trusted source.
