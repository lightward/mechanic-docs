# Events

In Mechanic, an **event** represents anything that happens. This could be an order being paid, or a customer record being created, or a fulfillment being delivered.

An event always has a [**topic**](topics.md), and usually contains **data**. Events may trigger any number of [**tasks**](../tasks/), resulting in any number of [**actions**](../actions/).

Events are fed into Mechanic by the responsible party – for events that are about things in Shopify, for example, the events come to Mechanic from Shopify itself.

{% hint style="info" %}
Incoming events may be selectively filtered out using [event filters](../../platform/events/filters.md).
{% endhint %}

