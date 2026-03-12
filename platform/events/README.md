# Events

In Mechanic, an [event](../../core/events/) represents something that happened — a new order, a scheduled timer firing, an incoming email, or a custom trigger. Events are the starting point for all automation: every task run begins with an event.

## How events work

Each event has a **topic** (like `shopify/orders/create`) and **data** (the payload from Shopify or another source). [Tasks](../../core/tasks/) declare interest in events by adding [subscriptions](../../core/tasks/subscriptions.md) that match specific topics. When an event arrives, Mechanic runs every task that subscribes to that event's topic.

## Event sources

Events can come from several places:

* **Shopify webhooks** — triggered by activity in your store (orders, customers, products, fulfillments, etc.)
* **Mechanic scheduler** — timed events like `mechanic/scheduler/daily` or `mechanic/scheduler/hourly`
* **User triggers** — manual runs initiated from the Mechanic UI via `mechanic/user/trigger`
* **Custom events** — events fired by tasks using the [Event action](../../core/actions/event.md), enabling multi-step workflows
* **Incoming email** — emails sent to your store's Mechanic address trigger `mechanic/emails/received`
* **Error events** — `mechanic/errors/task`, `mechanic/errors/action`, and `mechanic/errors/event` fire when something goes wrong

## In this section

* [Event topics](topics.md) — a complete index of all available event topics
* [Event filters](filters.md) — selectively skip events before they reach your tasks
