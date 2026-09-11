# Delinea PAM — Deployment

## Installation (MSI)

*(Requirements screenshots referenced in the original notes.)*

Before installing, check:

- **macOS agent requirements** — noted separately from the Windows/MSI requirements (see macOS/JAMF section below).
- **Privilege Manager server requirements** — what the server itself needs to run.
- Additional requirements to plan for:
    - Web server
    - Database server
    - Endpoints
    - Ports

## Installation (agent) — hardening

Once the agent is installed, it's worth locking down who can change it:

- On Windows computers, go to **Scheduled Jobs**.
- There's an option titled **"Restrict account permission on Agent services."**
- Activate this policy, and make sure the only group that can edit the agent is **Administrator**.

!!! important
    Without this, any local account with enough rights could tamper with the agent service — restricting edit rights to Administrator closes that gap.

## Installation (macOS agent & JAMF)

The macOS agent is made up of several components:

- Privilege Manager
- System extension
- Preferences pane
- Sudo plugin
- Service agent

**JAMF** is a company that makes software to manage Apple devices (Mac, iPhone, iPad, Apple TV) in organizations — i.e. an Apple device management (MDM) company. It's used to push and manage the Delinea macOS agent components across a fleet of Macs the same way other MDM profiles are deployed.

## Reverse proxy

See [Architecture](architecture.md) for why and how a reverse proxy is placed in front of the Privilege Manager server.
