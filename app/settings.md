---
description: Configure your Mechanic account, permissions, email, and integrations.
---

# Settings

The Settings page is organized into tabs. The **General** and **User** tabs cover the most common settings — the other tabs are for specific features you can explore as needed.

<figure><img src="../.gitbook/assets/mechanic_settings.jpg" alt="The Mechanic Settings page showing the General tab"><figcaption></figcaption></figure>

## General

* **System contact email** — receives Mechanic system notifications, like rate limit alerts or platform maintenance announcements. Supports multiple addresses separated by commas.
* **Read all orders** — by default, Mechanic accesses the last 60 days of order history. Enable this if you have tasks that need to search older orders. Learn more in [Read all orders](../platform/shopify/read-all-orders.md).
* **Subscription** — your current plan and billing status.

## User

* **Use Advanced mode by default** — open the [task editor](task-editor.md) in Advanced mode (for editing code) instead of Basic mode (for adjusting settings)
* **Show shop identity banner** — display a banner identifying the current shop (helpful when managing multiple stores)
* **Use dark theme for code editor** — apply a dark theme to all code editors in the app

## Permissions

Mechanic automatically detects the Shopify API [permissions](../core/tasks/permissions.md) your tasks need — you don't need to manage this manually. This tab shows which permissions are currently granted and which are required.

## Webhooks

Create [webhooks](../platform/webhooks.md) to trigger Mechanic tasks from external apps or systems. For example, you can create a webhook so that when a form is submitted on your website, Mechanic runs a task in response. Each webhook gets a unique URL you can POST data to.

## Email

* **Default outbound email** — the auto-assigned email address Mechanic uses for sending, with email verification status
* **Custom outbound email** — use your own domain, verified via DNS records. See [Custom email addresses](../platform/email/custom-email-domain.md).

## Email templates

Create reusable HTML [email templates](../platform/email/templates.md) for use in tasks. Templates support Liquid variables.

## Cache endpoints

Create URLs that external apps can visit to retrieve data saved by your tasks. Each endpoint maps a [cache](../platform/cache/) key to a URL that returns the current value as JSON.

## Event filters

Liquid scripts that run before tasks to decide whether an event should be processed. Returning `false` skips the event entirely — useful for filtering out test orders or pausing processing during maintenance. See [Event filters](../platform/events/filters.md).

## Authentication

Connect your Google, Airtable, and Slack accounts so your tasks can interact with those services. Credentials are encrypted and stored securely. See [Integrations](../platform/integrations/).
