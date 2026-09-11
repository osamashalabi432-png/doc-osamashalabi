# Proofpoint — Customizing Your Email Digest

An email digest batches up quarantined or held messages (spam, bulk mail, etc.) and delivers them to the user as a single periodic summary email, instead of letting each one arrive — or get silently dropped — separately. This gives end users a way to review and release messages that were held, without flooding their inbox.

## Setting a digest schedule

**Where:** `System` → `End User Services` → `Digest Schedule`

1. Turn the digest **on**.
2. Select the time you prefer it to be sent, using the green arrow to choose the delivery time.
3. Save the changes.

!!! warning
    Don't forget to save — schedule changes are not applied until you do.

## Using a custom sender (SMTP) profile

By default, digest emails come from Proofpoint's standard sending address. You can instead set a different **From** address for the digest:

1. Go to `System` → `Settings` → `SMTP`.
2. Click the **Profile** button.
3. Fill in the required sender information.
4. Go to `End User Services` → `Digest Settings`.
5. Select the digest schedule you just created so it uses the new SMTP profile.
