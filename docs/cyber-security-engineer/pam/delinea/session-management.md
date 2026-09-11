# Delinea PAM — Session Management

## Session monitoring & recording

Granting temporary/JIT access (see [Policies](policies.md)) covers *who* can get in — session monitoring covers what they *do* once they're in. PAM can:

- Record every privileged session (RDP, SSH, SQL)
- Provide keystroke-level audit logs
- Alert when suspicious actions happen
- Allow real-time supervision ("shadow" sessions)

This is what makes time-limited privileged access auditable rather than just time-limited: even during the access window, there's a full record of what was actually done, and an operator can watch a session live if something looks off.
