# GraphQL

**GraphQL** is Shopify's preferred API language – it's the way developers are encouraged to query for and submit Shopify data. Mechanic tasks frequently use GraphQL to efficiently retrieve and write data from Shopify.

{% hint style="warning" %}
Shopify REST is deprecated in Mechanic. All new tasks should use the GraphQL Admin API. See [Converting tasks from Shopify REST to GraphQL](../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migration guidance.
{% endhint %}

## Reading data with GraphQL

Use the [`shopify` Liquid filter](../liquid/filters/#shopify) to run a GraphQL query inline during a task run. The result is available immediately.

```liquid
{% capture query %}
  query {
    orders(first: 5, query: "financial_status:paid") {
      nodes {
        id
        name
        totalPriceSet {
          shopMoney {
            amount
          }
        }
      }
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{% for order in result.data.orders.nodes %}
  {{ order.name }}: {{ order.totalPriceSet.shopMoney.amount }}
{% endfor %}
```

Learn more: [Queries](basics/queries.md)

## Writing data with GraphQL

Use the [Shopify action](../../core/actions/shopify.md) to run GraphQL mutations. Mutations are queued during the task run and **performed after the script finishes**.

```liquid
{% action "shopify" %}
  mutation {
    tagsAdd(id: "gid://shopify/Order/1234567890", tags: ["reviewed"]) {
      node {
        id
      }
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

Learn more: [Mutations](basics/mutations.md)

## Learn more

* [Shopify Admin API GraphiQL explorer](basics/shopify-admin-api-graphiql-explorer.md) — test queries interactively
* [Queries](basics/queries.md) — reading data with GraphQL
* [Mutations](basics/mutations.md) — writing data with GraphQL
* [Pagination](basics/pagination.md) — handling paginated results
* [Bulk operations](bulk-operations.md) — processing large datasets
