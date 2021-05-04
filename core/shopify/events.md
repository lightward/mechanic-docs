# Responding to events

Shopify uses webhooks to notify apps like Mechanic about new activity. Mechanic supports every type of Shopify webhook in its set of [**Shopify event topics**](../../platform/events/topics.md#shopify). By setting up [**subscriptions**](../tasks/subscriptions.md) to these topics, a task may respond to any supported type of Shopify activity.

## Responding to changes in specific data

Shopify's "update" webhooks do not contain information about what piece of data has changed. \(For example, a product update webhook does not specify what attribute of the product has changed.\) For this reason, it's not possible to subscribe to changes in specific resource attributes \(like product SKUs, or order tags\).

If a task needs to react to a specific attribute change, the task must scan for and "remember" the original value of that attribute, so as to compare incoming updates with that remembered value. A task could use the [Cache](../actions/cache.md) action to store these values in the Mechanic cache, or it could use the [Shopify](../actions/shopify.md) action to save the remembered value in a metafield.

For an example implementation, see the [Auto-tag products when their variants change](https://usemechanic.com/task/auto-tag-products-when-their-skus-change) task.

