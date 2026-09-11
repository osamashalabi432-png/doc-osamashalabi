# FortiWeb — Architecture

## Authentication and access control

FortiWeb can authenticate end users against several external identity sources rather than only relying on local accounts. Supported methods include:

- RADIUS
- NTLM
- KDC
- SAML
- TACACS+

### ADFS

Active Directory Federation Services (AD FS) is a Microsoft solution that provides authentication across an organization, allowing users to log in once (single sign-on) and use that session across integrated services — rather than authenticating separately against each one.

!!! note "Placeholder"
    The deeper architecture topics below have not been written yet — source notes for these did not go beyond a section header. Verified content will be added here from official vendor documentation and first-hand lab/deployment notes: SSO/SAML configuration, access control methods, user tracking, and attacks on authentication.
