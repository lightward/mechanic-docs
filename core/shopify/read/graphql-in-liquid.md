# GraphQL in Liquid

Tasks may use the [shopify Liquid filter](../../../platform/liquid/filters.md#shopify) to convert GraphQL query strings into simple result objects, by sending the query to the [Shopify GraphQL Admin API](https://shopify.dev/docs/admin-api/graphql). The easiest way to build these queries is via the [Shopify Admin API GraphiQL explorer](https://shopify.dev/tools/graphiql-admin-api), which allows queries to be interactively constructed.

{% hint style="info" %}
The shopify filter does not support running mutations (i.e. writing Shopify data via GraphQL). To run mutations, use the [Shopify action](../../actions/shopify.md).
{% endhint %}

## Usage

The [shopify filter](../../../platform/liquid/filters.md#shopify) accepts a GraphQL query string, and returns everything back from Shopify's GraphQL admin API. This means that reading back GraphQL data is as easy as this:

```liquid
{% raw %}
{% capture query %}
  query {
    shop {
      name
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{% log result.data.shop.name %}
{% endraw %}
```

{% hint style="info" %}
The [shopify filter](../../../platform/liquid/filters.md#shopify) also supports GraphQL variables!
{% endhint %}

If you're working with multiple pages of data, you might use set up a forloop, using a cursor to retrieve page after page:

```liquid
{% raw %}
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
{% endraw %}
```

{% hint style="info" %}
You'll note that this code includes stub data when running during a preview event. This technique is extremely useful for generating [dynamic preview actions](../../tasks/previews/), by allowing you to exercise your entire task script.
{% endhint %}

The hardest part of using GraphQL in Mechanic is writing the query itself. :) For help with this, we recommend installing [Shopify's GraphiQL app](https://shopify-graphiql-app.shopifycloud.com/). It provides an environment where, using auto-complete and built-in documentation, you can rapidly build the right query for your task.

Note: GraphQL queries (excluding whitespace) are limited to 50,000 characters. That's a hard limit, enforced on Shopify's end – if you bump up against it, you'll need to adjust your query strategy to always stay under that limit. If you're saving large values to a metafield, for example, consider separating those values using GraphQL variables, keeping the query itself trim. Learn more about this scenario using the [Shopify action](../../actions/shopify.md#graphql-with-variables), or with the [shopify Liquid filter](../../../platform/liquid/filters.md#shopify).

## Use GraphQL when...

* ... you want to make things more efficient. GraphQL is fantastic for being really precise about what data you want, which makes your tasks run in less time: no more looping through collections to find your data, and no more downloading data you don't require.

## Don't use GraphQL when...

* ... it's easier and more readable to use Liquid objects, _unless_ performance becomes an issue. Ultimately, the most important thing is that your task works well tomorrow – and that includes making sure that whoever works on it next understands what you're doing. If that means using a quick-and-simple Liquid lookup over a moderately-more-complex GraphQL lookup, go for it.
* ... you find yourself staring at nested loops. Looping through all orders is one thing – it's quite another to loop through pages of orders _and_ loop through pages of line items _within each order_. For those scenarios, whenever possible, use a bulk operation.
