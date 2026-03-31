---
description: >-
  Send automated emails from Mechanic — templates, custom domains, DMARC, and receiving inbound email.
---

# Email

Mechanic email is powered by [Postmark](https://postmarkapp.com/).

## Sending email

Stores using Mechanic can send email using the [Email action](../../core/actions/email.md).

{% hint style="warning" %}
**Email approval is required.** Sending email with Mechanic requires manual approval from the Mechanic platform team. Until your account is approved, all Email actions will fail.

To request email approval, contact [team@usemechanic.com](mailto:team@usemechanic.com) and let us know you'd like to enable email sending for your store.
{% endhint %}

By default, emails are sent from a Mechanic-managed address on your behalf. You can configure a custom sender address using a [custom email domain](custom-email-domain.md).

### Features

* [**Email templates**](templates.md) — create reusable email templates with Liquid, configurable in Mechanic's settings
* [**Custom email addresses**](custom-email-domain.md) — send from your own domain instead of Mechanic's default
* [**DMARC**](dmarc.md) — configure DMARC alignment for improved deliverability
* **Attachments** — attach [generated files](../../core/actions/file-generators/) (PDFs, CSVs, etc.) to outgoing emails

{% hint style="info" %}
Common questions: [How do I send images with my emails?](../../faq/how-do-i-send-images-with-my-emails.md) · [How do I preview email attachments?](../../faq/how-do-i-preview-email-attachments.md)
{% endhint %}

## Receiving email

Stores using Mechanic can receive email using the [mechanic/emails/received](receiving-email.md) event topic. When an email is received at your store's Mechanic email address, Mechanic creates an event containing the email's subject, body, sender, and any attachments.

Learn more: [Receiving email](receiving-email.md)
