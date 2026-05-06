# Writing data

There is a single correct answer for writing data to Shopify: the [**Shopify**](../actions/shopify.md) action. :)

For high-volume writes, the Shopify action can start a [bulk operation mutation](../../resources/tutorials/bulk-operation-mutations.md), using a staged JSONL variables file and `bulkOperationRunMutation`.

For the shared read/write flow, see [Bulk operations](bulk-operations.md).
