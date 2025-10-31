# Tagging Shopify resources

Merchants commonly use Mechanic for adding or removing tags from customers, orders, products, and other Shopify resources.

When writing your task scripts, use [a Shopify action](../core/actions/integrations/shopify.md) to add or remove tags via the Shopify API.&#x20;

## GraphQL

Because the permission required varies based on what you're tagging, make sure to render a representative node ID during preview mode to avoid "TagsAdd access denied" errors. In this approach, we use stub data to create dynamic [preview actions](../core/tasks/previews/).

```javascript
{% if event.preview %}
  {% capture customer_json %}
    {
      "admin_graphql_api_id": "gid://shopify/Customer/12345"
    }
  {% endcapture %}

  {% assign customer = customer_json | parse_json %}
{% endif %}

{% action "shopify" %}
  mutation {
    tagsAdd(
      id: {{ customer.admin_graphql_api_id | json }}
      tags: ["some", "new", "tags"]
    ) {
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}

{% action "shopify" %}
  mutation {
    tagsRemove(
      id: {{ customer.admin_graphql_api_id | json }}
      tags: ["tags", "to", "remove"]
    ) {
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

### Logging the results

GraphQL does not automatically log the results of a tagging operation. However, it's easy to include the final tags for a resource, in the results of your GraphQL mutation – very useful for proving that your mutation is doing what you expect, and also for allowing you to refer back to the current tagged state of the resource.

```javascript
{% action "shopify" %}
  mutation {
    tagsAdd(
      id: {{ customer.admin_graphql_api_id | json }}
      tags: ["some", "new", "tags"]
    ) {
      node {
        ... on Customer {
          tags
        }
      }
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

Note the triple dots: the `... on <TYPE>` syntax marks an inline fragment. Replace "Customer" with the name of the GraphQL type you're working with (e.g. "Product", or "Order). Read more from Shopify: [GraphQL / Advanced Concepts / Inline Fragments](https://shopify.dev/concepts/graphql/advanced#inline-fragments).
