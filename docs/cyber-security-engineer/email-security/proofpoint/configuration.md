# Proofpoint — Technical Configuration

Core configuration tasks for Proofpoint's Email Protection component: protecting executives from display-name impersonation, and enabling deep inspection of URLs and attachments inside messages.

## Imposter Email Display Name Repository (SCSS)

Attackers commonly spoof the **display name** of a high-value target (a CEO or CFO, for example) rather than their real address, hoping the recipient never checks the underlying email address. Proofpoint's **Supernatural Classifier Spam Score (SCSS)** engine, combined with a display-name repository, is built specifically to catch this pattern for a protected list of executives.

**Where:** `Email Protection` → `Spam Detection` → `Settings` → `General`

1. Enable **SCSS**. If the option isn't available, contact Proofpoint support to have it enabled on the account.
2. Build a list of the executives who have previously been targeted by imposter/spoofing attempts — anyone likely to be impersonated should be on this list.
3. Go to **Imposter Display Names** in the same section.
4. Click **Add**, then enter the name of the user who is likely to be spoofed.
5. Use **Add and New** to keep adding entries. If the list of display names is long, it can be imported from a CSV file instead of being entered one by one.

!!! note
    SCSS has a learning period of **9 days** and requires a minimum of **2,000 messages** before it reaches full effectiveness. Expect reduced accuracy until both thresholds are met.

## URL Defense Configuration

URL Defense protects against malicious links by rewriting URLs in email so that, when a user clicks, the link is first checked against Proofpoint's cloud-based reputation service before the user is redirected to the real destination.

**Where:** `Email Protection` → `Targeted Attack Protection` → `URL Defense` → `Settings`

1. Enable **URL Defense**.
2. Go to **URL Rewrite**.
3. Select **Rewrite URLs in All Messages** — this provides the highest level of security, since every URL is routed through Proofpoint's cloud servers for verification before the user reaches it.
4. Decide how the rewritten URL is displayed to the end user.
5. Define any exceptions for domains or senders that should bypass rewriting.

!!! note
    URL Defense applies to URLs inside **HTML attachments**, as long as the attachment is directly attached to the email rather than embedded inside an archive (e.g. a `.zip`).

!!! tip
    Supported protocols for rewritten links are **HTTP, HTTPS, and FTP**.

## Email Attachment Defense Configuration

Attachment Defense inspects file attachments for malicious content before delivery.

**Where:** `Email Protection` → `Targeted Attack Protection` → `Attachment Defense` → `Settings`

1. Enable Attachment Defense.
2. To exclude specific traffic from scanning, create a policy and attach it to a **policy route**:
   - Go to `Email Protection` → `Targeted Attack Protection` → `Attachment Defense` → `Policies`.
   - Click the **Default** button in the policy list.
   - Click **Edit Rules** to modify what the policy scans or excludes.

!!! warning
    The source notes stop here — later detail on Attachment Defense (specific rule types, verdict handling) was not captured and is not included.
