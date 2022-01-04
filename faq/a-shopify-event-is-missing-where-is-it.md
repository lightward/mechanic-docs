# A Shopify event is missing. Where is it?

{% hint style="info" %}
This FAQ is about Shopify events specifically. For a more general treatment of this question, refer to [Why don't I see any events in my task's activity?](why-dont-i-see-any-events-in-my-tasks-activity.md).
{% endhint %}

Start by checking [status.mechanic.dev](https://status.mechanic.dev) and [shopifystatus.com](https://shopifystatus.com) for service interruptions. (If you're not signed up for Mechanic status notifications, this is a good time to take care of that!)

If the event is well and truly missing, email [team@usemechanic.com](mailto:team@usemechanic.com) for further assistance.

**⚠️** Please note: these investigations usually conclude that Shopify has simply failed to deliver that specific event webhook. Shopify does not offer a strict guarantee on webhook delivery (see [their documentation](https://shopify.dev/apps/webhooks#implement-reconciliation-jobs)), which means that the solution is usually to ensure tasks can recover when events go missing.

To learn more about this, see [Reconciling missing events](../core/shopify/events/reconciling-missing-events.md).
