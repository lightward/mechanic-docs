# Events

In Mechanic, an **event** represents anything that happens. This could be an order being paid, or a customer record being created, or a fulfillment being delivered.

An event always has a [**topic**](topics.md), and **data** (even if the data is null/nil). Event attributes may be referenced in Liquid using the [**Event object**](../../platform/liquid/objects/event.md).

Events may trigger any number of [**tasks**](../tasks/), resulting in any number of [**actions**](../actions/).

Events are fed into Mechanic by the responsible party – for events that are about things in Shopify, for example, the events come to Mechanic from Shopify itself.

{% hint style="info" %}
Incoming events may be selectively skipped using [event filters](../../platform/events/filters.md).
{% endhint %}
