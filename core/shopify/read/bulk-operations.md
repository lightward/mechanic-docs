# Bulk operations

Mechanic supports Shopify's [bulk operations GraphQL API](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api), allows developers to submit a query to Shopify for asynchronous processing, and making the results available to the task once complete.

This approach dodges the issues inherent in synchronous methods of reading data \(like GraphQL via the shopify filter, or REST via Liquid objects\). Unlike these methods, the bulk operations API does not exhaust Shopify API limit for your Mechanic account, and therefore does not slow down other tasks. It also does not require any special logic for pagination, since Shopify handles all data collection.

{% hint style="warning" %}
Shopify only permits apps to run a single bulk operation at a time, per store. Requests to create another bulk operation, while one is already running, will result in an error.
{% endhint %}

## Usage

### Creating a bulk operation

Use the [Shopify](../../actions/shopify.md) action to execute a bulkOperationRunQuery mutation \(see [Shopify's tutorial](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api#bulk-query-overview)\). Mechanic will detect this mutation, and will begin monitoring the bulk operation in progress.

{% hint style="info" %}
We've found that processing objects is easiest when you can identify each object by its type. To that end, try including `__typename` in the list of selections for each node, right alongside `id`.
{% endhint %}

### Subscribing to the results

Add a [subscription](../../tasks/subscriptions.md) to the `mechanic/shopify/bulk_operation` event topic.

Once Mechanic detects that the bulk operation has been completed, the platform will automatically re-invoked the same task with a `mechanic/shopify/bulk_operation` event, containing the bulk operation's data, when the bulk operation is complete.

### Accessing the results

When processing a mechanic/shopify/bulk\_operation event, the task will have access to an [environment variable](../../tasks/code/environment-variables.md) called `bulkOperation`, containing all attributes of the bulk operation \([docs](https://shopify.dev/docs/admin-api/graphql/reference/bulk-operations/bulkoperation)\).

The set of objects returned by the bulk operation is made available as `bulkOperation.objects`, allowing you to scan returned data immediately, using an expression like `{% for object in bulkOperation.objects %}`.

The JSON objects returned from bulk operation queries each include a `"__parentId"` attribute for connected objects, containing the parent object's ID. To make managing task scripts easier, Mechanic allows you to simply call `{{ object.__parent }}` to look up an object's parent.

{% hint style="info" %}
We've found that processing objects is easiest when you can identify each object by its type. To that end, try updating your bulk operation query to include `__typename` in the list of selections for each node, right alongside `id`.
{% endhint %}

## Example

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/user/trigger
mechanic/shopify/bulk_operation
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Code" %}
```javascript
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
{% endtab %}
{% endtabs %}

### More examples

* [Task: Calculate total quantities purchased by SKU](https://usemechanic.com/task/calculate-total-quantities-purchased-by-sku)
* [Task: Sync order timeline comments to the customer note](https://usemechanic.com/task/sync-order-timeline-comments-to-the-customer-note)
* [Task: Auto-tag orders with their tracking numbers](https://usemechanic.com/task/auto-tag-orders-with-their-tracking-numbers)

## Use bulk operations when...

* ... your task needs to collect and process a lot of data. Tasks responding to bulk operations operate with a higher memory allowance than other tasks, decreasing the chances of your task being terminated for memory exhaustion.
* ... paginating for data would be too complicated. Pagination in GraphQL can be painful, especially when using nested resources.

## Don't use bulk operations when...

* ... you only need a little bit of data. Use the [shopify](../../../platform/liquid/filters.md#shopify) filter instead.
* ... you're responding to a Shopify event, and the data you need comes along with the event data. Use [Liquid objects](liquid-objects.md) instead.

