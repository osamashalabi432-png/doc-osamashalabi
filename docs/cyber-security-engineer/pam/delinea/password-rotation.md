# Delinea PAM — Password Rotation

## Automatically rotating passwords

A privileged password that never changes is a standing risk — anyone who ever saw it can keep using it indefinitely. PAM ensures privileged passwords:

- Rotate regularly
- Rotate **immediately after use**
- Are never reused
- Are never shared between employees

Rotating immediately after each use is the important behavior here: it turns "checked out a password" into a one-time credential rather than a persistent shared secret, which limits how long a leaked or over-shared password stays useful.
