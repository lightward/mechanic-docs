# A Shopify event is missing. Where is it?

Start by checking [status.mechanic.dev](https://status.mechanic.dev/) and [shopifystatus.com](https://shopifystatus.com/) for service interruptions. \(If you're not signed up for Mechanic status notifications, this is a good time to take care of that!\)

If the event is well and truly missing, email [team@usemechanic.com](mailto:team@usemechanic.com) for further assistance.

Please note: these investigations usually conclude that Shopify has simply failed to deliver that specific event webhook. Shopify does not offer a strict guarantee on webhook delivery \(see [their documentation](https://shopify.dev/apps/webhooks#implement-reconciliation-jobs)\), which means that the solution is usually to ensure tasks are resilient in the presence of missing events.

To learn more about this, see Reconciling missing events.



* **⚠️ Recall that Shopify doesn't guarantee webhook deliverability.** During Shopify service interruptions, webhooks can be severely delayed. And, we've seen some cases in which Shopify completely fails to deliver webhooks – very rare, but it does happen. For critical tasks, we suggest subscribing to a [scheduler event topic](../platform/events/topics.md#scheduler), and using those events to scan for orders/customers/etc that may have been missed. Make sure these tasks can also handle late arrivals of relevant Shopify events, because a scheduled run of the task may process an updated Shopify resource before the Shopify event _from_ that update comes in.

