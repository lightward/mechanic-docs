# Bulk operations

Mechanic supports Shopify's bulk operations GraphQL API, allows developers to submit a query to Shopify for asynchronous processing, and making the results available to the task once complete.

This approach dodges the issues inherent in synchronous methods of reading data (like GraphQL via the shopify filter, or REST via Liquid objects). Unlike these methods, the bulk operations API does not exhaust Shopify API limit for your Mechanic account, and therefore does not slow down other tasks. It also does not require any special logic for pagination, since Shopify handles all data collection.

{% hint style="info" %}
This article reviews Mechanic's support for bulk operations. If this feature is new to you, start by reading Shopify's tutorial on the bulk operations API: [https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api)
{% endhint %}

{% hint style="warning" %}
Shopify only permits apps to run a single bulk operation at a time, per store. Actions that create another bulk operation, while one is already running, will return an error.

This area deserves improvement! To discuss the future of this behavior, visit the related feature request: [https://mechanic.canny.io/futures/p/bulk-operation-retries](https://mechanic.canny.io/futures/p/bulk-operation-retries)
{% endhint %}

## Usage

### Creating a bulk operation

Use the [Shopify](../../actions/shopify.md) action to execute a bulkOperationRunQuery mutation (see [Shopify's tutorial](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api#bulk-query-overview)). Mechanic will detect this mutation, and will begin monitoring the bulk operation in progress.

### Subscribing to the results

Add a [subscription](../../tasks/subscriptions.md) to mechanic/shopify/bulk\_operation.

Once Mechanic detects that the bulk operation has been completed, the platform will automatically re-invoked the same task with an event having the topic "mechanic/shopify/bulk\_operation", containing the bulk operation's data, when the bulk operation is complete. (As with [mechanic/actions/perform](../../../techniques/responding-to-action-results.md), Mechanic will only invoke the current task when the bulk operation completes; no other task will be notified.)

### Accessing the results

When processing a mechanic/shopify/bulk\_operation event, the task will have access to an [environment variable](../../tasks/code/environment-variables.md) called `bulkOperation`, containing all attributes of the bulk operation ([docs](https://shopify.dev/docs/admin-api/graphql/reference/bulk-operations/bulkoperation)).

The set of objects returned by the bulk operation is made available as `bulkOperation.objects`, allowing you to scan returned data immediately, using an expression like `{% for object in bulkOperation.objects %}`.

In most cases, every object that has an ID will appear as a separate object, in the same set of objects. For example, if a product and five variants are returned, there will be six objects returned – the variants are not nested inside of the product object.

The JSON objects returned from bulk operation queries each include a `"__parentId"` attribute for connected objects, containing the parent object's ID. To make managing task scripts easier, Mechanic allows you to simply call `{{ object.__parent }}` to look up an object's parent.

{% hint style="info" %}
Because all objects are returned as peers in a flat set, we've found that processing objects is easiest when you can easily identify each object by its type. To that end, try including `__typename` in the list of selections for each node, right alongside `id`.

This technique allows the array of objects to be quickly filtered by type:

```javascript
{% raw %}
{% assign customers = bulkOperation.objects | where: "__typename", "Customer" %}
{% endraw %}
```
{% endhint %}

## Example

{% tabs %}
{% tab title="Subscriptions" %}
```
mechanic/user/trigger
mechanic/shopify/bulk_operation
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Code" %}
```javascript
{% raw %}
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
{% endraw %}
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

* ... you only need a little bit of data. Use the [shopify](../../../platform/liquid/filters/#shopify) filter instead.
* ... you're responding to a Shopify event, and the data you need comes along with the event data. Use [Liquid objects](liquid-objects.md) instead.
