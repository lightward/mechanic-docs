# Why are my tasks delayed or not running?

In general, there are two reasons why a task might be delayed, or might not be appearing to run:

1. Your Mechanic queue is backed up
2. The upstream service (like Shopify) is delayed in sending notifications to Mechanic

{% hint style="info" %}
Using Mechanic for a business-critical purpose? Learn about [monitoring Mechanic](../techniques/monitoring.md).
{% endhint %}

## If your Mechanic queue is backed up...

... then you'll see a steady stream of incoming activity, in your account's activity log, but you might not see activity corresponding to the very latest events in your store. Mechanic has [some concurrency limits](../core/runs/concurrency.md) that determine how much can happen in your account at once, and if you reach those limits, processing of new events will be delayed until Mechanic catches up.

If it's useful, you can consider disabling unnecessary tasks, which let each emerging task run fail instantly instead of running normally. It won't clear the event queue, but it'll let the event queue run more quickly. More on that here: [How do I stop a large batch of runs?](how-do-i-stop-a-large-batch-of-runs.md)

## If upstream service is delayed in sending notifications to Mechanic...

... then you won't see anything appearing in Mechanic's activity log. If that service is Shopify, you might see something reported [on Shopify's status page](https://www.shopifystatus.com/).

In most cases, delays are resolved in time, and the delayed events are later on sent to Mechanic, where they can be processed.

{% hint style="warning" %}
In very rare cases, Shopify may erroneously miss an event entirely. This is a documented behavior of Shopify's webhook system. To learn more about this, see [Reconciling missing events](../core/shopify/events/reconciling-missing-events.md).
{% endhint %}

### A note about update events

So as to avoid sending out floods of notifications, Shopify waits a few seconds before sending notifications for update events, to see if any other updates occur that should be included in the notification.

For example, if you change a product's title, then save it, then notice a mistake and change the title again, Shopify may only send a single update instead of two. This means that update events take a little longer to arrive than other events.

[Read more about this from Shopify](https://community.shopify.com/c/API-Announcements/Upcoming-improvements-to-Webhooks/m-p/398576)
