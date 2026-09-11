# Identity & Access Management

## User Account Control (UAC)

UAC is a feature that enforces enhanced access control and ensures that all services and applications execute in non-administrator accounts by default. It's the mechanism behind the "Do you want to allow this app to make changes to your device?" prompt — it forces a deliberate elevation step before code runs with administrative rights, rather than letting everything run elevated silently.

Access UAC via `Control Panel > User Accounts > Change User Account Control Setting`.

## Password Policies

Password policies configured through the local policy editor help enforce complex, strong passwords for user accounts. Example policies that meaningfully raise the bar for attackers:

- Passwords must contain both uppercase and lowercase characters.
- Check passwords against leaked or already-hacked databases or a dictionary of compromised passwords.
- After 6 failed login attempts within 15 minutes, the account remains locked for at least 1 hour.

Access Password Policies via `Local Group Policy Editor > Security settings > Account Policies > Password policy`.

## Setting a Lockout Policy

A lockout policy protects against password guessing by automatically locking an account after a number of invalid login attempts — without it, an attacker (or automated tool) can attempt passwords indefinitely against an account.

!!! important
    Configure `Local Security Policy > Windows Settings > Account Policies > Account Lockout Policy` to lock accounts out after three invalid attempts.
