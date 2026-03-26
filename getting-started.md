---
description: Install Mechanic and set up your first Shopify automation in minutes.
---

# Getting started

Mechanic is a Shopify automation and development platform with hundreds of ready-to-use tasks — and a full development environment for building your own.

{% stepper %}
{% step %}

## Install Mechanic

Install Mechanic from the [Shopify App Store](https://apps.shopify.com/mechanic). Once installed, you'll find it in your Shopify admin under **Apps > Mechanic**.

{% endstep %}
{% step %}

## Find a task in the task library

The fastest way to get started is to browse the [task library](https://tasks.mechanic.dev/) — a collection of hundreds of pre-built automations covering common Shopify workflows like auto-tagging, scheduled emails, inventory management, and more.

To install a task, browse the [task library](https://tasks.mechanic.dev/) and click **Try this task** — or use the [**Add task**](https://admin.shopify.com/apps/mechanic/tasks/add) page inside the app. Review the task's description, configure any options, and click **Save** to activate it.

Many tasks have configurable options (like thresholds, tag names, or email templates) that let you customize behavior without writing any code.

{% hint style="info" %}
Can't find exactly what you need? You can [request a task](resources/task-library/requesting.md) for the library — top-voted requests are regularly built.
{% endhint %}

{% endstep %}
{% step %}

## See it in action

Once you save a task, it's live — Mechanic will run it automatically when the matching event occurs. For example, a task that subscribes to `shopify/orders/create` will run every time a new order is placed.

To confirm it's working, open the [**Events**](app/events.md) page in the Mechanic app. You'll see incoming events and the task runs they trigger, including any actions that were performed or errors that occurred.

{% endstep %}
{% endstepper %}

From here, you can:

* **Customize** — adjust the task's options or code to fit your workflow
* **Browse more tasks** — install additional tasks from the [task library](https://tasks.mechanic.dev/)
* **Go custom** — build your own task from scratch (see below)

## Go custom

If the task library doesn't cover your use case, Mechanic is a full development platform. Tasks are written in [Liquid](platform/liquid/basics/) with Mechanic-specific extensions for data manipulation, API access, and workflow composition. You get direct [GraphQL access](core/shopify/read/graphql-in-liquid.md) to the Shopify Admin API, [HTTP actions](core/actions/http.md) for calling any external service, and the ability to chain tasks together through [custom events](core/actions/event.md) — all without managing servers or deployments.

* [I need something custom!](custom.md) — an overview of the path to a custom task, whether you're a developer or need to find one
* [Your first custom task](resources/tutorials/your-first-custom-task.md) — a step-by-step tutorial for building a task from scratch

## Get help

* [Slack community](resources/slack.md) — ask questions and share solutions with other Mechanic users
* [Hire a Mechanic Partner](hire-a-developer.md) — find a developer to build or customize your task
* [Contact support](support.md) — how to reach the Mechanic support team
