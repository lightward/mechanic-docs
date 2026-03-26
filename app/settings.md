---
description: Configure your Mechanic account, permissions, email, and integrations.
---

# Settings

The Settings page is organized into tabs covering different areas of your Mechanic configuration.

## General

* **System contact email** — the email address that receives Mechanic system notifications (rate limit warnings, service announcements). Supports multiple addresses separated by commas.
* **Read all orders** — by default, Mechanic accesses the last 60 days of order history. Enable this to access your full order history. Learn more in [Read all orders](../platform/shopify/read-all-orders.md).
* **Subscription** — displays your current plan and billing status.

## User

Personal preferences for your Mechanic experience:

* **Use Advanced mode by default** — open the [task editor](task-editor.md) in Advanced mode instead of Basic mode
* **Show shop identity banner** — display a banner identifying the current shop (helpful when managing multiple stores)
* **Use dark theme for code editor** — apply a dark theme to all code editors in the app

## Permissions

Manage the Shopify API [permissions](../core/tasks/permissions.md) Mechanic uses:

* **Required access scopes** — permissions needed by your active tasks, with indicators showing which are currently granted
* **Available access scopes** — all permissions your Shopify app has access to
* **Custom Shopify API token** — for advanced use cases, provide a custom access token from a private or custom Shopify app

Mechanic automatically detects which scopes your tasks need — no manual scope management is required in most cases.

## Webhooks

Create [webhooks](../platform/webhooks.md) to send events to Mechanic from external systems:

* **Name** — a friendly label for the webhook
* **Event topic** — the event identifier (e.g., `user/form/submitted`)
* **Event data mode** — Auto (detect format automatically) or Full request (capture entire HTTP body and headers)
* **Webhook URL** — the auto-generated URL to POST data to

## Email

Configure outbound email:

* **Default outbound email** — the auto-assigned email address Mechanic uses for sending, with DNS/SPF/DKIM status
* **Custom outbound email** — use your own domain for outbound email, verified via DNS records. See [Custom email addresses](../platform/email/custom-email-domain.md).

## Email templates

Create reusable HTML [email templates](../platform/email/templates.md) for use in tasks. Templates support Liquid variables and are written in a full code editor.

## Cache endpoints

Create public HTTP endpoints that expose [cached data](../platform/cache/) from your tasks. Each endpoint maps a cache key to a URL that returns the current value as JSON.

## Event filters

Write Liquid scripts that run before tasks to decide whether an event should be processed. Returning `false` skips the event entirely. Useful for filtering out test orders, pausing processing during maintenance, or skipping events by topic. See [Event filters](../platform/events/filters.md).

## Authentication

Manage OAuth connections for third-party [integrations](../platform/integrations/):

* **Google** — for Google Sheets and Google Drive actions
* **Airtable** — for Airtable actions
* **Slack** — for Slack actions

Connected accounts store encrypted access tokens so your tasks can interact with these services without embedding API keys.
