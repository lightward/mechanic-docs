# Bulk operations

Fetching or writing large amounts of Shopify data can exhaust normal API limits, slow down other tasks, and consume too much task time or memory.

To solve this general problem, Shopify offers the bulk operations GraphQL API. Bulk operations let you submit work to Shopify for asynchronous processing, and Shopify stores the results in a JSONL file.

{% hint style="info" %}
For a review of how Mechanic uses bulk operations, start here: [Shopify / Bulk operations](../../core/shopify/bulk-operations.md).
{% endhint %}

Mechanic supports both Shopify bulk operation types:

* `bulkOperationRunQuery`, for fetching large data sets.
* `bulkOperationRunMutation`, for running the same mutation once per JSONL variables line.

Both operation types are delivered back to the originating task using `mechanic/shopify/bulk_operation`. Use `bulkOperation.type` to distinguish `QUERY` from `MUTATION`.

For mutation results, remember that a completed operation means Shopify finished processing the file. It does not mean every line succeeded. Each JSONL result line should be checked for GraphQL errors and mutation `userErrors`.

## Great resources for learning Shopify GraphQL bulk operations

### Key concepts

* `bulkOperationRunQuery`
* `bulkOperationRunMutation`
* Staged JSONL uploads for bulk mutations
* Polling your operation's status (don't worry, Mechanic takes care of this for you)
* Data retrieval and JSONL
* Rate limits
* Operation restrictions

{% hint style="success" %}
[Shopify: bulkOperationRunQuery](https://shopify.dev/docs/api/admin-graphql/latest/mutations/bulkOperationRunQuery)
{% endhint %}

{% hint style="success" %}
[Shopify: bulkOperationRunMutation](https://shopify.dev/docs/api/admin-graphql/latest/mutations/bulkOperationRunMutation)
{% endhint %}

{% hint style="success" %}
[Mechanic tutorial: Running bulk operation mutations](../../resources/tutorials/bulk-operation-mutations.md)
{% endhint %}
