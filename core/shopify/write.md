# Writing data

To create or update a Shopify record from a task, use the [**Shopify**](../actions/shopify.md) action with a GraphQL mutation. Mechanic performs the action after the task finishes.

For high-volume writes, the Shopify action can start a [bulk operation mutation](../../resources/tutorials/bulk-operation-mutations.md), using a staged JSONL variables file and `bulkOperationRunMutation`.

For the shared read/write flow, see [Bulk operations](bulk-operations.md).
