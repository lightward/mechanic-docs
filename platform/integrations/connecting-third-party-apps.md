---
description: "Learn how to connect Mechanic to any third-party app using HTTP APIs and webhooks."
---

# Connecting third-party apps

Mechanic's [built-in integrations](./) cover popular services like Slack, Airtable, and Google Sheets — but they're just the beginning. Mechanic can connect to virtually any service that offers an HTTP API or outbound webhooks, making it a universal automation hub for your Shopify store.

## Built-in integrations at a glance

### OAuth-managed integrations

These integrations are authenticated via **Settings > Authentication** in Mechanic, so you don't need to manage API keys yourself.

| Integration | What it does |
|---|---|
| [Airtable](airtable.md) | Read/write records, comments, and schemas |
| [Google Drive](google-drive-and-google-sheets.md) | Create and upload files, manage folders |
| [Google Sheets](google-drive-and-google-sheets.md) | Create spreadsheets, append rows, export data |
| [Slack](slack.md) | Send messages and post to channels |

### Webhook-based integrations

These integrations push events into Mechanic automatically. Configure them to send data to a Mechanic webhook URL.

| Integration | What it does |
|---|---|
| [Appstle Subscriptions](appstle-subscriptions.md) | Subscription lifecycle events |
| [Judge.me](judge.me.md) | Review events |
| [Locksmith](locksmith.md) | Access control events |
| [Report Toaster](report-toaster.md) | Report and update events |
| [Shopify Flow](shopify-flow.md) | Bi-directional Flow triggers |

## Connecting apps without a built-in integration

Any app with an HTTP API or webhook support can be connected to Mechanic using two general patterns: **receiving data** (ingress) and **sending data** (egress).

### Receiving data from external apps

#### Webhooks

Most third-party apps can send outbound webhooks. Create a webhook in Mechanic (**Settings > Webhooks**), then point the external app at the resulting URL. Incoming data arrives as a `user/` event that your tasks can subscribe to.

{% hint style="info" %}
See [Webhooks](../webhooks.md) for setup details, and [Creating a Mechanic webhook](../../resources/tutorials/creating-a-mechanic-webhook.md) for a step-by-step tutorial.
{% endhint %}

#### Scheduled polling

For apps that don't support outbound webhooks, use a scheduler subscription (e.g. `mechanic/scheduler/hourly`) paired with an [HTTP action](../../core/actions/http.md) to pull data on a schedule.

#### Inbound email

Some services can send data via email. Mechanic can receive and process inbound emails as events. See [Receiving email](../email/receiving-email.md).

### Sending data to external apps

#### HTTP action

The [HTTP action](../../core/actions/http.md) supports GET, POST, PUT, PATCH, and DELETE requests with custom headers, authentication, and request bodies. It handles JSON, form-encoded, and multipart payloads.

#### FTP action

The [FTP action](../../core/actions/ftp.md) uploads files to external SFTP and FTP servers — useful for EDI integrations, bulk data feeds, and partners that accept file drops.

#### Files and cache endpoints

Generate files (PDFs, CSVs, ZIPs) using [file generator actions](../../core/actions/files.md), then make them available at a public URL via [cache endpoints](../cache/endpoints.md) for external services to fetch.

### Handling authentication

Most third-party APIs require authentication. The HTTP action supports several common methods:

* **API key / token in headers** — The most common approach. Add an `Authorization` or custom header with your API key.
* **Basic auth** — Supported natively by the HTTP action. Supply a username and password.
* **Query string tokens** — Some APIs accept a key as a URL parameter.

{% hint style="warning" %}
OAuth-based APIs that require interactive user authorization are only supported for Mechanic's [built-in OAuth integrations](./#oauth). You cannot perform an OAuth authorization flow for arbitrary third-party services.
{% endhint %}

## Example: Connecting a subscription app

This example shows how to connect a subscription management app (like Recharge or Appstle) that has its own API and webhook support.

### Receiving subscription events

1. In Mechanic, go to **Settings > Webhooks** and create a new webhook — for example, named `recharge`.
2. Copy the webhook URL and configure it in the subscription app's webhook settings.
3. Create a Mechanic task subscribed to the resulting event topic (e.g. `user/webhook/recharge`).

### Calling the subscription app's API

Use the HTTP action with an API key header to call the app's API:

```liquid
{% action "http" %}
  {
    "method": "GET",
    "url": "https://api.example-app.com/subscriptions/{{ subscription_id }}",
    "headers": {
      "X-Api-Key": "{{ options.api_key__required }}"
    }
  }
{% endaction %}
```

### Updating Shopify in response

Combine the external API call with Shopify actions to keep your store in sync:

```liquid
{% action "shopify" %}
  mutation {
    tagsAdd(
      id: "gid://shopify/Customer/{{ customer_id }}"
      tags: ["active-subscriber"]
    ) {
      node { id }
      userErrors { field message }
    }
  }
{% endaction %}
```

## Limitations and best practices

* **No arbitrary OAuth** — Interactive OAuth authorization flows are only available for built-in integrations. For other services, use API keys or tokens.
* **Execution limits** — Mechanic tasks run within memory and time limits. For large data sets, use [bulk operations](../../core/shopify/read/bulk-operations.md) or paginated polling.
* **Validate webhook payloads** — When receiving data from external services, consider [securing your Mechanic webhooks](../../techniques/securing-mechanic-webhooks.md) to verify authenticity.

{% hint style="info" %}
For advanced patterns — including JSON Web Signatures, AWS request signatures, and paginated API calls — see [Working with external APIs](../../techniques/working-with-external-apis/).
{% endhint %}

## Next steps

* Browse the [task library](https://tasks.mechanic.dev/) for existing integrations with third-party services
* [I need something custom!](../../custom.md) — Get help building a custom integration
* Join the [Mechanic Slack community](../../resources/slack.md) to ask questions and share solutions
