# Concurrency

In general, Mechanic will process as many [**runs**](./) simultaneously as possible. This means that multiple tasks subscribing to the same event topic are very likely to execute simultaneously, when such an event occurs.

To protect the health of the system and to ensure performance for every store on the platform, Mechanic have several **concurrency** limits, defining the conditions in which Mechanic will perform runs simultaneously.

{% hint style="info" %}
In most cases, an inefficient run queue is best addressed by combining or reorganizing tasks, improving [Shopify API usage efficiency](../shopify/api-rate-limit.md) (converting REST requests to GraphQL is often helpful), or by making judicious use of [event filters](../../platform/events/filters.md).

It can be also useful to temporarily disable a task responsible for the backup; doing so will cause Mechanic to instantly fail its enqueued runs when they come up for processing, but it will not fail those task runs ahead of time.
{% endhint %}

## Limits

Each store's Mechanic account has a fixed run queue size. This limit controls how many runs Mechanic will perform simultaneously for your store. With a limit of 5, this could mean 5 events, or 5 tasks, or 1 event and 2 tasks and 2 actions, or any other combination of runs. Additional runs will be performed as the preceding runs complete.

{% hint style="info" %}
Related FAQ: [Can my Mechanic concurrency limit be raised?](../../faq/can-my-mechanic-concurrency-limit-be-raised.md)
{% endhint %}

## Tips

* Use GraphQL to query Shopify, to keep your data usage efficient. (To learn more, see [Interacting with Shopify](../shopify/).)
* For options for ordering execution of runs, see [Ordering](ordering.md).
