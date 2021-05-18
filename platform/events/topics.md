---
description: A complete index
---

# Event topics

Mechanic supports a variety of [**event topics**](../../core/events/topics.md). This page is a complete index of pre-defined topics, organized by **domain** and **subject**.

## Mechanic

### Actions

* **mechanic/actions/perform** Occurs when an action has been performed, regardless of its success or failure. A task may subscribe to this topic to be notified when each of its actions have been performed, so that the task may then respond to the results.

### Emails

* **mechanic/emails/received** Occurs when Mechanic receives an email sent to the store's dedicated email address. \(A store at example.myshopify.com can receive email at example@mail.usemechanic.com.\)

### Scheduler

* **mechanic/scheduler/10min** Runs every 10 minutes.
* **mechanic/scheduler/15min** Runs every 15 minutes.
* **mechanic/scheduler/20min** Runs every 20 minutes. \(You see where this is going.\)
* **mechanic/scheduler/30min** Runs every 30 minutes.
* **mechanic/scheduler/hourly** Runs every hour, on the hour.
* **mechanic/scheduler/daily** Runs at midnight, in the store's local timezone.
* **mechanic/scheduler/monday** Runs every Monday at midnight, in the store's local timezone. \(Note: this would be at the end of Sunday evening\). Each named weekday is supported.

{% hint style="info" %}
More specific times can be achieved by subscribing to an event topic with a delay \(e.g. "mechanic/scheduler/daily+5.hours"\). To learn more, see [Subscriptions](../../core/tasks/subscriptions.md).
{% endhint %}

### User

Not to be confused with events in the User domain, the mechanic/user event subject describes ways that the user expresses their intent – to Mechanic – to run the task.

* **mechanic/user/trigger** When used, adds a "Run task" button to the task itself. This button generates a new event, to which only this task will respond.
* **mechanic/user/text** When used, adds a "Run task" button to the task itself. This button prompts the user with a multi-line text box. When submitted, an event is generated, to which only this task will respond. The event contains the user's input in its data, making user's input available in `{{ event.data }}`.



{% hint style="info" %}
Shopify events are received from Shopify, directly, and contain event data provided by Shopify. For a reference on what each event contains, see [Shopify's Webhook documentation](https://shopify.dev/docs/admin-api/rest/reference/events/webhook).

Tasks subscribing to Shopify events receive [Liquid environment variables](../../core/tasks/code/environment-variables.md), containing data from the webhook.
{% endhint %}

## Shopify

### Cart

* shopify/carts/create
* shopify/carts/update

### Checkout

* shopify/checkouts/create
* shopify/checkouts/update
* shopify/checkouts/paid
* shopify/checkouts/delete

### Collections

* shopify/collections/create
* shopify/collections/update
* shopify/collections/delete

### Customer saved searches

* shopify/customer\_groups/create 
* shopify/customer\_groups/update 
* shopify/customer\_groups/delete 

### Customers

* shopify/customers/create
* shopify/customers/delete
* shopify/customers/disable
* shopify/customers/enable
* shopify/customers/update

### Disputes

* shopify/disputes/create
* shopify/disputes/update

### Draft orders

* shopify/draft\_orders/create
* shopify/draft\_orders/delete
* shopify/draft\_orders/update

### Fulfillment events

* shopify/fulfillment\_events/create
* shopify/fulfillment\_events/delete

### Fulfillments

* shopify/fulfillments/create
* shopify/fulfillments/update

### Inventory Items

* shopify/inventory\_items/create
* shopify/inventory\_items/update
* shopify/inventory\_items/delete

### Inventory Levels

* shopify/inventory\_levels/connect
* shopify/inventory\_levels/update
* shopify/inventory\_levels/disconnect

### Locations

* shopify/locations/create
* shopify/locations/update
* shopify/locations/delete

### Order transactions

* shopify/order\_transactions/create

### Orders

* shopify/orders/cancelled
* shopify/orders/create
* shopify/orders/delete
* shopify/orders/fulfilled
* shopify/orders/paid
* shopify/orders/partially\_fulfilled
* shopify/orders/updated
* shopify/orders/edited

### Products

* shopify/products/create
* shopify/products/delete
* shopify/products/update

### Refunds

* shopify/refunds/create

### Shop

* shopify/shop/update

### Themes

* shopify/themes/create
* shopify/themes/delete
* shopify/themes/publish
* shopify/themes/update

## User

The User event domain is for custom, user-generated events, having any subject and verb \(e.g. "user/foo/bar"\). As with all events, a User event topic must use the standard three-part topic form, but only the "user/" prefix is mandatory.

Mechanic allows developers several ways to generate custom User events:

* The [Event action](../../core/actions/event.md) can be used with any User event topic
* [Webhooks](../webhooks.md) may be configured to generate events using any User event topic

## Third-party

### Judge.me

* **judgeme/review/created** Occurs when new Judge.me reviews are received. Requires enabling the Mechanic integration within Judge.me; see [Judge.me's integration announcement](https://blog.judge.me/blog/new-integration-mechanic-app).

### Additional apps

Mechanic invites third-party app developers to develop Mechanic integrations, making it easy for developers to use our services together. If you're an app developer interested in this, email [team@usemechanic.com](mailto:team@usemechanic.com).

{% hint style="info" %}
Third-party services can often be connected to Mechanic using [webhooks](../webhooks.md).
{% endhint %}

