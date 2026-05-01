---
description: >-
  Respond to Shopify webhooks in Mechanic — subscribe to store activity and handle changes to orders, products, customers, and more.
---

# Responding to events

Shopify uses webhooks to notify apps like Mechanic about new activity. Mechanic supports Shopify webhooks through native [**Shopify event topics**](../../../platform/events/topics.md#shopify) and [custom Shopify webhooks](../../../platform/shopify/custom-webhooks.md). By setting up [**subscriptions**](../../tasks/subscriptions.md), a task may respond to supported Shopify activity.

{% hint style="warning" %}
Note that Shopify does not strictly guarantee webhook delivery. See [Reconciling missing events](reconciling-missing-events.md) for more on this subject.
{% endhint %}

## How Shopify events reach Mechanic

There are two paths Shopify webhooks can take into Mechanic. Most tasks use the first; reach for the second when you need filtering, payload customization, or metaobject events.

### Native subscriptions

When a task subscribes to a `shopify/...` topic (e.g. `on: shopify/products/update`), Mechanic registers and maintains the corresponding webhook subscription with Shopify automatically. Deliveries are unfiltered, full-shape payloads. This is the path used by every task in the [task library](https://tasks.mechanic.dev/) and the default for every example in these docs. The complete topic list lives in [Event topics](../../../platform/events/topics.md#shopify).

### Custom Shopify webhooks

When you need filtered deliveries (only active products, only orders with a specific tag), slimmer payloads (`include_fields`), Shopify-side metafield filtering or delivery (`metafield_namespaces`, `metafields`), or **metaobject events** (which aren't in the native topic set), configure a [Custom Shopify webhook](../../../platform/shopify/custom-webhooks.md). Each custom webhook routes Shopify deliveries onto a `user/...` topic of your choosing.

## Responding to changes in specific data

Shopify's "update" webhooks do not contain information about what piece of data has changed. (For example, a product update webhook does not specify what attribute of the product has changed.) For this reason, it's not possible to subscribe to changes in specific resource attributes (like product SKUs, or order tags).

If a task needs to react to a specific attribute change, the task must scan for and "remember" the original value of that attribute, so as to compare incoming updates with that remembered value. A task could use the [Cache](../../actions/cache.md) action to store these values in the Mechanic cache, or it could use the [Shopify](../../actions/shopify.md) action to save the remembered value in a metafield.

For an example implementation, see the [Auto-tag products when their variants change](https://usemechanic.com/task/auto-tag-products-when-their-skus-change) task.

##
