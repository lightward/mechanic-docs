# Bulk operations

Mechanic supports Shopify's bulk operations GraphQL API, allowing tasks to ask Shopify to process large jobs asynchronously and then return to the task when the job completes.

Bulk operations are useful for reading large amounts of Shopify data, and for running the same mutation many times using JSONL input. In both cases, Shopify produces a JSONL result file, and Mechanic makes that result available to the task.

{% hint style="info" %}
This article reviews Mechanic's support for bulk operations. For Shopify's own reference, see [bulkOperationRunQuery](https://shopify.dev/docs/api/admin-graphql/latest/mutations/bulkOperationRunQuery) and [bulkOperationRunMutation](https://shopify.dev/docs/api/admin-graphql/latest/mutations/bulkOperationRunMutation).
{% endhint %}

{% hint style="info" %}
Shopify's concurrency limits vary by API version. In API versions `2026-01` and higher, Shopify allows up to five bulk query operations and up to five bulk mutation operations at the same time per shop. In earlier API versions, Shopify allows one operation of each type (`bulkOperationRunQuery` or `bulkOperationRunMutation`) at a time per shop. If Shopify rejects a new operation because of a concurrency limit, the Shopify action will return a GraphQL error.
{% endhint %}

## Bulk operation types

| Operation | Use it for | Mechanic behavior |
| --------- | ---------- | ----------------- |
| `bulkOperationRunQuery` | Reading a large set of Shopify data | Mechanic detects the returned bulk operation, monitors it, and invokes the same task with `mechanic/shopify/bulk_operation` when it completes. |
| `bulkOperationRunMutation` | Running one mutation for each JSONL variables line uploaded to Shopify | Mechanic detects the returned bulk operation, monitors it, and invokes the same task with `mechanic/shopify/bulk_operation` when it completes. |

The completed bulk operation object includes a `type` field. Use `bulkOperation.type` when a task needs to branch between query results and mutation results:

```liquid
{% if bulkOperation.type == "MUTATION" %}
  {% log message: "Bulk mutation complete" %}
{% elsif bulkOperation.type == "QUERY" %}
  {% log message: "Bulk query complete" %}
{% endif %}
```

## Creating a query bulk operation

Use the [Shopify](../actions/shopify.md) action to execute a `bulkOperationRunQuery` mutation. Mechanic will detect the returned bulk operation, and will begin monitoring it.

## Creating a mutation bulk operation

Use the [Shopify](../actions/shopify.md) action to execute a `bulkOperationRunMutation` mutation. This requires a `stagedUploadPath`, which comes from first creating a Shopify staged upload and uploading a JSONL variables file.

Shopify allows most Admin API mutations in `bulkOperationRunMutation`, except recursive bulk operation mutations such as `bulkOperationRunMutation` and `bulkOperationRunQuery` themselves. Shopify also limits the mutation string to one connection field. Mechanic still applies the Shopify action's mutation safety checks; if a mutation is not allowed in Mechanic, the action run will fail with an error.

Because staged uploads require both Shopify GraphQL and an HTTP multipart file upload, mutation bulk operations usually use a multi-step Mechanic flow:

1. Use the Shopify action to run `stagedUploadsCreate`.
2. Subscribe to `mechanic/actions/perform` and use the [HTTP action](../actions/http.md) to upload the JSONL file to Shopify.
3. Subscribe to `mechanic/actions/perform` again and use the Shopify action to run `bulkOperationRunMutation`.
4. Subscribe to `mechanic/shopify/bulk_operation` to process the completed result file.

For a complete walkthrough, see [Running bulk operation mutations](../../resources/tutorials/bulk-operation-mutations.md).

## Subscribing to the results

Add a [subscription](../tasks/subscriptions.md) to `mechanic/shopify/bulk_operation`.

Once Mechanic detects that the bulk operation has completed, the platform will automatically re-invoke the same task with an event having the topic `mechanic/shopify/bulk_operation`, containing the bulk operation's data. As with [mechanic/actions/perform](../../techniques/responding-to-action-results.md), Mechanic will only invoke the current task when the bulk operation completes; no other task will be notified.

## Accessing the results

When processing a `mechanic/shopify/bulk_operation` event, the task will have access to an [environment variable](../tasks/code/environment-variables.md) called `bulkOperation`, containing all attributes of the bulk operation ([Shopify docs](https://shopify.dev/docs/api/admin-graphql/latest/objects/BulkOperation)).

The set of objects returned by the bulk operation is made available as `bulkOperation.objects`, allowing you to scan returned data immediately:

```liquid
{% for object in bulkOperation.objects %}
  {% log object: object %}
{% endfor %}
```

For query bulk operations, every object that has an ID usually appears as a separate object in the same set. For example, if a product and five variants are returned, there will be six objects returned; the variants are not nested inside of the product object.

The JSON objects returned from bulk operation queries each include a `"__parentId"` attribute for connected objects, containing the parent object's ID. To make managing task code easier, Mechanic allows you to call `{{ object.__parent }}` to look up an object's parent.

For mutation bulk operations, each object represents the GraphQL response for one JSONL input line. Completed bulk mutations can still contain per-line GraphQL errors or mutation `userErrors`, so inspect each result before treating the write as successful. Shopify includes fields like `data`, `errors`, and `__lineNumber` in the result JSONL.

{% hint style="info" %}
Because query bulk operation objects are returned as peers in a flat set, processing query results is easiest when you can identify each object by its type. Include `__typename` in the list of selections for each node, right alongside `id`.

This technique allows the array of objects to be quickly filtered by type:

```liquid
{% assign customers = bulkOperation.objects | where: "__typename", "Customer" %}
```
{% endhint %}

## Query example

**Subscriptions**

```
mechanic/user/trigger
mechanic/shopify/bulk_operation
```

**Code**

```liquid
{% if event.topic == "mechanic/user/trigger" %}
  {% capture bulk_operation_query %}
    query {
      customers {
        edges {
          node {
            __typename
            id
            email
          }
        }
      }
    }
  {% endcapture %}

  {% action "shopify" %}
    mutation {
      bulkOperationRunQuery(
        query: {{ bulk_operation_query | json }}
      ) {
        bulkOperation {
          id
          status
        }
        userErrors {
          field
          message
        }
      }
    }
  {% endaction %}
{% elsif event.topic == "mechanic/shopify/bulk_operation" %}
  {% assign customers = bulkOperation.objects | where: "__typename", "Customer" %}
  {% assign emails = customers | map: "email" %}
  {% log emails: emails %}
{% endif %}
```

## Mutation example

For a complete mutation example, see [Running bulk operation mutations](../../resources/tutorials/bulk-operation-mutations.md).

## More examples

* [Task: Calculate total quantities purchased by SKU](https://usemechanic.com/task/calculate-total-quantities-purchased-by-sku)
* [Task: Sync order timeline comments to the customer note](https://usemechanic.com/task/sync-order-timeline-comments-to-the-customer-note)
* [Task: Auto-tag orders with their tracking numbers](https://usemechanic.com/task/auto-tag-orders-with-their-tracking-numbers)

## Use bulk operations when...

* ... your task needs to collect and process a lot of data.
* ... paginating for data would be too complicated.
* ... you need to run the same Shopify-supported and Mechanic-allowed mutation many times from a JSONL variables file.
* ... your task needs the higher memory allowance available to task runs responding to `mechanic/shopify/bulk_operation`.

## Don't use bulk operations when...

* ... you only need to read a little bit of data. Use the [shopify](../../platform/liquid/filters/#shopify) filter instead.
* ... you only need to write a small number of records. Use the [Shopify action](../actions/shopify.md) directly instead.
* ... you're responding to a Shopify event, and the data you need comes along with the event data. Use [Liquid objects](read/liquid-objects.md) instead.
* ... you need action results immediately in the same task run. Bulk operation results are delivered later, using `mechanic/shopify/bulk_operation`.
