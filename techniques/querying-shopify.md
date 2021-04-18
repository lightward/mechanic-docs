# Querying Shopify

Mechanic supports three methods for fetching data from Shopify:

* [Liquid objects](https://secure.helpscout.net/docs/5e28a1c704286364bc9443ab/article/5e28a1e104286364bc9443ea/#liquid-objects)
* [GraphQL](https://secure.helpscout.net/docs/5e28a1c704286364bc9443ab/article/5e28a1e104286364bc9443ea/#graphql)
* [GraphQL with bulk operations](https://secure.helpscout.net/docs/5e28a1c704286364bc9443ab/article/5e28a1e104286364bc9443ea/#graphql-with-bulk-operations)

{% hint style="info" %}
This article is about reading data from Shopify. Use the [Shopify](../core-concepts/actions/types/shopify.md) action to write data.
{% endhint %}

## Liquid objects <a id="liquid-objects"></a>

### How to use Liquid objects

Mechanic comes with a rich set of [Liquid objects](../platform/liquid/objects/), in much the same way [that Shopify does](https://shopify.dev/docs/themes/liquid/reference/objects). The main difference: Mechanic's implementation gives you data from Shopify's REST admin API, whereas Shopify's implementation deviates from API data in some cases.

Each task script is given a set of global variables to work with, out of the box. Mechanic's task script editor will tell you which ones are available. For example, for a task responding to a `shopify/orders/` event, you might see this:

![](https://s3.amazonaws.com/helpscout.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e12c7d3a7e9ae69bd2/5e28a1e1a248a.png)

The [cache](../platform/liquid/objects/cache.md), [event](../platform/liquid/objects/event.md), [options](../platform/liquid/objects/options.md), and [shop](../platform/liquid/objects/shopify/shop.md) objects are always available for tasks; the [order](../platform/liquid/objects/shopify/order.md) object \(as in this example\) contains the order to which the current event relates.

Use [Mechanic's Liquid object documentation](../platform/liquid/objects/) to discover what data is available for each Liquid object.

### Use Liquid objects when...

* ... your task's event gives you an automatic object containing the data you need. For example, for a task responding to a "shopify/customers/" event, you'll get an automatic `customer` variable. Feel free to use this variable to get to additional data, like `customer.orders.first.name`.
* ... you know you're not going to need to load an enormous amount of data. For example, a `{% for customer in shop.customers %}` loop is just fine if you know your store will have only hundreds or thousands of customers.
* ... when it's easy to get to the right data, allowing future versions of you to easily understand what you were doing. ;\) There are plenty of scenarios where it's easier to use Liquid objects than it is to use GraphQL, and if you can do so without accidentally downloading too much data \(see above\), go for it.

### Don't use Liquid objects when...

* ... there's a more efficient way to get to what you need. For example, getting all orders tagged "sale" via Liquid objects will require loading in _all_ orders, and then using Liquid to skip orders that don't have that tag. This takes a long time, _and_ it's unnecessary, because GraphQL supports searching for orders by tag.

## GraphQL <a id="graphql"></a>

### How to use GraphQL

Mechanic provides [a Liquid filter called "shopify"](../platform/liquid/filters.md#shopify), specifically for executing your hand-written GraphQL query, and returning everything back from Shopify's GraphQL admin API. This means that reading back GraphQL data is as easy as this:

```javascript
{% capture query %}
  query {
    shop {
      name
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{{ result.data.shop.name }}
```

Or, if you're working with multiple pages of data, you might use set up a forloop, using a cursor to retrieve page after page:

```javascript
{% assign cursor = nil %}
{% assign total_inventory = 0 %}

{% for n in (0..100) %}
  {% capture query %}
    query {
      products(
        first: 250
        after: {{ cursor | json }}
      ) {
        pageInfo {
          hasNextPage
        }
        edges {
          cursor
          node {
            totalInventory
          }
        }
      }
    }
  {% endcapture %}

  {% assign result = query | shopify %}

  {% if event.preview %}
    {% capture result_json %}
      {
        "data": {
          "products": {
            "edges": [
              {
                "node": {
                  "totalInventory": -4
                }
              }
            ]
          }
        }
      }
    {% endcapture %}

    {% assign result = result_json | parse_json %}
  {% endif %}

  {% for product_edge in result.data.products.edges %}
    {% assign product = product_edge.node %}
    {% assign total_inventory = total_inventory | plus: product.totalInventory %}
  {% endfor %}

  {% if result.data.products.pageInfo.hasNextPage %}
    {% assign cursor = result.data.products.edges.last.cursor %}
  {% else %}
    {% break %}
  {% endif %}
{% endfor %}
```

{% hint style="info" %}
You'll note that this code includes stub data when running during a preview event. This technique is extremely useful for generating [dynamic preview actions](../core-concepts/tasks/previews/), by allowing you to exercise your entire task script.
{% endhint %}

Writing data back to Shopify is also possible, using [the Shopify action](../core-concepts/actions/types/shopify.md):

```javascript
{% action "shopify" %}
  mutation {
    tagsAdd(
      id: "gid://shopify/Customer/1234567890"
      tags: ["foo", "bar", "baz"]
    ) {
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

This also means that the hardest part of using GraphQL in Mechanic is writing the query itself. :\) For help with this, we recommend installing [Shopify's GraphiQL app](https://shopify-graphiql-app.shopifycloud.com/). It provides an environment where, using auto-complete and built-in documentation, you can rapidly build the right query for your task.

Note: GraphQL queries \(excluding whitespace\) are limited to 50,000 characters. That's a hard limit, enforced on Shopify's end – if you bump up against it, you'll need to adjust your query strategy to always stay under that limit. If you're saving large values to a metafield, for example, consider separating those values using GraphQL variables, keeping the query itself trim \(see [GraphQL with variables](../core-concepts/actions/types/shopify.md#graphql-with-variables).\)

### Use GraphQL when...

* ... you want to make things more efficient. GraphQL is fantastic for being really precise about what data you want, which makes your tasks run in less time: no more looping through collections to find your data, and no more downloading data you don't require.

### Don't use GraphQL when...

* ... it's easier and more readable to use Liquid objects, _unless_ performance becomes an issue. Ultimately, the most important thing is that your task works well tomorrow – and that includes making sure that whoever works on it next understands what you're doing. If that means using a quick-and-simple Liquid lookup over a moderately-more-complex GraphQL lookup, go for it.
* ... you find yourself staring at nested loops. Looping through all orders is one thing – it's quite another to loop through pages of orders _and_ loop through pages of line items _within each order_. For those scenarios, whenever possible, use a bulk operation.

## GraphQL with bulk operations <a id="graphql-with-bulk-operations"></a>

The techniques described above require a task to manage all data fetching by itself. While manageable for most situations, fetching large amounts of data can exhaust the Shopify API limit for your Mechanic account, slowing down other tasks, and running the risk of consuming too much time or memory for your Mechanic workers. \(It's also a bit of a headache to manually perform pagination, as is the case for standard GraphQL.\)

To solve this general problem, Shopify offers [the bulk operations GraphQL API](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api). This API allows you to submit a query to Shopify for processing, the results to be stored elsewhere to be retrieved once the query is complete.

Mechanic supports this API, with a few extras to make life easier:

1. When you submit a bulk operation using Mechanic, your task is automatically re-invoked with a mechanic/shopify/bulk\_operation event, containing the bulk operation's data, when the bulk operation is complete.
2. When processing a mechanic/shopify/bulk\_operation event, your task script will have access to a variable called `bulkOperation`, containing all attributes of the bulk operation \([docs](https://help.shopify.com/en/api/graphql-admin-api/reference/object/bulkoperation)\).
3. The set of objects returned by the bulk operation is made available as `bulkOperation.objects`, allowing you to scan returned data immediately, using an expression like `{% for object in bulkOperation.objects %}`.
4. The JSON objects returned from bulk operation queries each include a `"__parentId"` attribute for connected objects, containing the parent object's ID. To make managing task scripts easier, Mechanic allows you to simply call `{{ object.__parent }}` to look up an object's parent.

### Usage notes

1. Your task should subscribe to one additional event topic: mechanic/shopify/bulk\_operation. Set up your task script to watch for this event topic intentionally, using the `bulkOperation` variable to access the operation's results.
2. Use the "bulkOperationRunQuery" GraphQL mutation to submit your query to Shopify, referring to [Shopify's documentation](https://shopify.dev/tutorials/perform-bulk-operations-with-admin-api#bulk-query-overview), and using the examples below as a guide.
3. We've found that processing objects is easiest when you can identify each object by its type. To that end, try including `__typename` in the list of selections for each node, right alongside `id`.
4. As of 2019-08-30, Shopify limits each store to one bulk operation at a time, per app. While this restriction is in place, you can only perform one bulk operation at a time with Mechanic.

### Example usage

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

### Example tasks

* [Task: Calculate total quantities purchased by SKU](https://usemechanic.com/task/calculate-total-quantities-purchased-by-sku)
* [Task: Sync order timeline comments to the customer note](https://usemechanic.com/task/sync-order-timeline-comments-to-the-customer-note)
* [Task: Auto-tag orders with their tracking numbers](https://usemechanic.com/task/auto-tag-orders-with-their-tracking-numbers)

