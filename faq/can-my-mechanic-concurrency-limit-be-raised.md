# Can my Mechanic concurrency limit be raised?

Perhaps! This question can come up when the Mechanic queue for a particular store becomes backlogged, delaying task activity. Queue concurrency is controlled by each store's Mechanic [**concurrency limit**](../core/runs/concurrency.md).

{% hint style="info" %}
In most cases, an inefficient run queue is best addressed by combining or reorganizing tasks, improving [Shopify API usage efficiency](../core/shopify/api-rate-limit.md) (converting REST requests to GraphQL is often helpful), or by making judicious use of [event filters](../platform/events/filters.md).

It can be also useful to temporarily disable a task responsible for the backup; doing so will cause Mechanic to instantly fail its enqueued runs when they come up for processing, but it will not fail those task runs ahead of time.
{% endhint %}

The limit can be evaluated and adjusted on a case-by-case basis with the platform support team. Contact [team@usemechanic.com](mailto:team@usemechanic.com) with your shop domain and as many specifics about your scenario as possible, and we can discuss from there.
