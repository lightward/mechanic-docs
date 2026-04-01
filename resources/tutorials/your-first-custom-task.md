---
description: >-
  Build your first Mechanic task from scratch — a step-by-step tutorial for auto-tagging high-value Shopify orders.
---

# Your first custom task

This tutorial walks you through building a complete Mechanic task from scratch. By the end, you'll have a working task that automatically tags orders over $100 with a "high-value" tag.

## Prerequisites

* A Shopify store with [Mechanic installed](https://apps.shopify.com/mechanic)
* Basic familiarity with what Mechanic does (see the [Introduction](../../README.md))

## What you'll build

When a new order comes in with a total over $100, your task will automatically add the tag "high-value" to it. This is a common pattern — tagging resources based on their properties — and it touches all the core concepts: events, subscriptions, Liquid code, previews, and actions.

## Step 1: Create a new task

1. Open Mechanic in your Shopify admin
2. Click the "Add task"  button (or navigate to the "Add task" page)
3. Click the "New blank task" button
4. Give your task a name: **Auto-tag high-value orders**

You'll see the task editor with several sections: subscriptions, code, and a preview panel.

## Step 2: Add a subscription

A [subscription](../../core/tasks/subscriptions.md) tells Mechanic which [events](../../core/events/) your task should respond to. Since you want to react to new orders, add this subscription:

```text
shopify/orders/create
```

This means your task will run every time Shopify sends a new order event to Mechanic. Each event has a [topic](../../core/events/topics.md) — in this case `shopify/orders/create` — and Mechanic matches it to tasks with a matching subscription.

{% hint style="info" %}
You can browse all available event topics in the [Event topics](../../platform/events/topics.md) reference. Shopify topics correspond to Shopify webhooks; Mechanic also has its own topics for schedules, errors, and more.
{% endhint %}

## Step 3: Write the task code

Mechanic tasks are written in [Liquid](../../platform/liquid/basics/), a template language created by Shopify. When your task runs, Mechanic provides [environment variables](../../core/tasks/code/environment-variables.md) — for a `shopify/orders/create` subscription, you automatically get an `order` variable containing the order data from the webhook.

Paste this into the **Code** section:

```liquid
{% if event.preview %}
  {% assign order = hash %}
  {% assign order["admin_graphql_api_id"] = "gid://shopify/Order/1234567890" %}
  {% assign order["total_price"] = "150.00" %}
{% endif %}

{% assign threshold = 100 %}

{% assign order_total = order.total_price | times: 1.0 %}

{% if order_total > threshold %}
  {% action "shopify" %}
    mutation {
      tagsAdd(
        id: {{ order.admin_graphql_api_id | json }}
        tags: ["high-value"]
      ) {
        userErrors {
          field
          message
        }
      }
    }
  {% endaction %}
{% endif %}
```

Let's break this down piece by piece.

### Preview stub data

```liquid
{% if event.preview %}
  {% assign order = hash %}
  {% assign order["admin_graphql_api_id"] = "gid://shopify/Order/1234567890" %}
  {% assign order["total_price"] = "150.00" %}
{% endif %}
```

[Previews](../../core/tasks/previews/) are how Mechanic shows you what your task will do — and how Mechanic figures out what Shopify permissions your task needs. During preview, the Shopify API is not available, so you provide [stub data](../../core/tasks/previews/stub-data.md): a fake order with a realistic ID and a price above the threshold.

The `hash` keyword creates an empty object. You then assign properties to it, replacing the `order` variable that would normally come from the event.

{% hint style="warning" %}
The stub data ID must look like a real Shopify resource ID (e.g. `gid://shopify/Order/1234567890`). Mechanic reads this ID during preview to determine which Shopify permissions to request — in this case, `write_orders` for tagging an order. You can also explicitly declare permissions using the [`{% permissions %}`](../../platform/liquid/tags/permissions.md) tag.
{% endhint %}

### Comparing the order total

```liquid
{% assign threshold = 100 %}
{% assign order_total = order.total_price | times: 1.0 %}

{% if order_total > threshold %}
```

The `order.total_price` value from Shopify's webhook is a string like `"150.00"`. The `times: 1.0` filter converts it to a number so you can compare it. If the total exceeds the threshold, the task proceeds to tag the order.

### The Shopify action

```liquid
{% action "shopify" %}
  mutation {
    tagsAdd(
      id: {{ order.admin_graphql_api_id | json }}
      tags: ["high-value"]
    ) {
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

The `{% action "shopify" %}` tag defines a [Shopify action](../../core/actions/shopify.md) — it tells Mechanic to make a GraphQL API call to Shopify. Inside the tag, you write a GraphQL mutation. The [`tagsAdd`](https://shopify.dev/docs/api/admin-graphql/latest/mutations/tagsAdd) mutation adds tags to any taggable resource.

The `| json` filter safely formats the order's GraphQL ID as a JSON string. Always include `userErrors` in your mutations — this is how Shopify reports problems.

{% hint style="info" %}
Actions are not performed immediately. Mechanic collects all actions from a task run, then performs them in sequence after the task finishes rendering. Learn more in [Actions](../../core/actions/).
{% endhint %}

## Step 4: Check the preview

After pasting the code, look at the preview panel on the right side of the editor. You should see a Shopify action containing your `tagsAdd` mutation, with the stub order ID and the "high-value" tag. This confirms two things:

1. Your task logic is working correctly
2. Mechanic knows your task needs the `write_orders` Shopify permission

If the preview is empty, double-check that your stub data sets `total_price` to a value above the threshold.

## Step 5: Save the task

Click **Save**. Mechanic will:

1. Parse your subscriptions and register for the `shopify/orders/create` webhook
2. Analyze the preview to determine required Shopify permissions
3. Prompt you to approve any new permissions it needs (like `write_orders`)

After saving, your task is live. The next time an order is placed with a total over $100, Mechanic will tag it with "high-value".

## Step 6: Test it

Place a test order in your store with a total over $100. Once Shopify processes the order, it will send a webhook to Mechanic, and your task will run automatically.

To see the results, open Mechanic's **Events** page. You'll see the incoming `shopify/orders/create` event, and you can expand it to see the task run and the action run that tagged the order. You can also verify the tag was applied by checking the order in Shopify admin.

## What you've learned

In this tutorial, you've worked with all of Mechanic's core concepts:

* **Events** — things that happen (a new order being created)
* **Subscriptions** — how a task declares interest in certain events
* **Task code** — Liquid that reads event data and decides what to do
* **Previews and stub data** — how tasks communicate their intent and permissions
* **Actions** — side effects the task produces (tagging an order via GraphQL)

## Getting help

{% hint style="info" %}
Mechanic support covers the platform and tasks from the [task library](../task-library/). Custom tasks you build yourself (including AI-generated tasks) are not covered by support. For help with custom task logic, ask in the [Mechanic Slack community](../slack.md) or [hire a developer](../../hire-a-developer.md).
{% endhint %}

## Next steps

* [Practicing writing tasks](practicing-writing-tasks.md) — more exercises to build your skills
* [Writing a high-quality task](../../techniques/writing-a-high-quality-task.md) — patterns for production-ready tasks
* [Task library](../task-library/) — hundreds of pre-built tasks to learn from and customize
* [Core Concepts: Tasks](../../core/tasks/) — full reference for task configuration
