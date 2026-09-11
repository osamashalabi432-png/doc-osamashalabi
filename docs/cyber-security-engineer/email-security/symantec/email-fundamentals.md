# Symantec — Email Fundamentals

Before configuring any email security product, it helps to know what an email message actually is under the hood — how it's formatted, what protocols move it around, and what the different "body" formats mean for filtering and security.

## Types of email body

| Type | Description | Use Case |
|---|---|---|
| Plain Text | No formatting | Security alerts, system logs, command-line tools |
| HTML Email | Styled emails with colors, fonts, links, images | Business mail, marketing newsletters |
| Multipart Email | Both a Plain Text and an HTML version included | Best practice for compatibility |

## Email structure (RFC 5322 standard)

RFC 5322 defines the standard structure of an internet email message: a set of header fields followed by the message body.

```text
Header Section
--------------
From:
To:
Subject:
Date:
Message-ID:
MIME-Version:
Content-Type:

Body Section
--------------
Message content (text / HTML / files)
```

## MIME structure example

MIME (Multipurpose Internet Mail Extensions) is what allows a single email to carry multiple content types — for example, a plain-text and an HTML version of the same message, or file attachments — inside one message, separated by a boundary marker.

```text
Content-Type: multipart/alternative; boundary=xyz

--xyz
Content-Type: text/plain
Hello, this is plain text.

--xyz
Content-Type: text/html
<b>Hello, this is HTML version!</b>
```

## How email works (flow overview)

```text
Mail Client (Outlook / Gmail / iPhone)
        ↓ SMTP
Mail Server (MTA: Postfix, Exim, Sendmail)
        ↓ DNS Lookup (MX record)
Receiving Mail Server
        ↓ IMAP / POP3
User Inbox
```

## Main email protocols

| Protocol | Purpose |
|---|---|
| SMTP (Port 25 / 465 / 587) | Send email |
| IMAP (Port 143 / 993) | Read email (server-based sync) |
| POP3 (Port 110 / 995) | Download email to client |
