# Shopify

The **Shopify** action sends requests to the [Shopify admin API](https://shopify.dev/docs/admin-api). It supports both REST and GraphQL requests.

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](shopify.md#graphql) going forward. The [product](../../platform/liquid/objects/shopify/product.md) and [variant](../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

{% hint style="info" %}
In Mechanic, writing data to Shopify must happen using an action. While the Shopify action is usually the right choice, the [HTTP](http.md) action can also be used for this purpose, by manually configuring authentication headers.

To learn more, see [Interacting with Shopify](../shopify/).
{% endhint %}

## Options

This action has several usage styles, each with a different set of constraints on action options.

### GraphQL

This usage style invokes the [Shopify GraphQL Admin API](https://shopify.dev/docs/admin-api/graphql). In this style, a single GraphQL query string is supplied as the action options. The [action](../../platform/liquid/tags/action.md) tag has specific support for this action type, allowing this string to be provided as the contents of an action block.

{% hint style="info" %}
To prepare complex query inputs, use the [graphql\_arguments](../../platform/liquid/filters/#graphql_arguments) Liquid filter.
{% endhint %}

{% tabs %}
{% tab title="Liquid" %}
```liquid
{% action "shopify" %}
  mutation {
    customerCreate(
      input: {
        email: "test@example.com"
      }
    ) {
      customer {
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
{% endtab %}
{% endtabs %}

### GraphQL with variables

This usage style invokes the [Shopify GraphQL Admin API](https://shopify.dev/docs/admin-api/graphql), and supports combining GraphQL queries with [GraphQL variables](https://graphql.org/learn/queries/#variables). This can be useful for re-using queries with multiple inputs, and is critical when dealing with very large pieces of input. Because GraphQL queries (excluding whitespace) are limited in length to 50k characters, GraphQL variables can be used in cases when large inputs (like Base64-encoded images) need to be submitted.

| Option      | Description                                              |
| ----------- | -------------------------------------------------------- |
| `query`     | Required; a string containing a GraphQL query            |
| `variables` | Required; a JSON object mapping variable names to values |

#### Basic example

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% capture query %}
  mutation DeleteProduct($productId: ID!) {
    productDelete(
      input: {
        id: $productId
      }
    ) {
      userErrors {
        field
        message
      }
    }
  }
{% endcapture %}

{% action "shopify" %}
  {
    "query": {{ query | json }},
    "variables": {
      "productId": "gid://shopify/Product/1234567890"
    }
  }
{% endaction %}
```
{% endtab %}
{% endtabs %}

#### Complex example

This example shows how the query and variables may be built up separately, and provided to the action using concise tag syntax.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% assign metafield_owner_id = "gid://shopify/Customer/507332001849" %
{% assign metafield_value = hash %}
{% assign metafield_value["foo"] = "bar" %}

{% capture query %}
  mutation MetafieldsSet($metafields: [MetafieldsSetInput!]!) {
    metafieldsSet(metafields: $metafields) {
      metafields {
        key
        namespace
        value
        createdAt
        updatedAt
      }
      userErrors {
        field
        message
        code
      }
    }
  }
{% endcapture %}

{% assign metafield = hash %}
{% assign metafield["ownerId"] = metafield_owner_id %}
{% assign metafield["namespace"] = "demo" %}
{% assign metafield["key"] = "demo" %}
{% assign metafield["type"] = "json" %}
{% assign metafield["value"] = metafield_value | json %}
{% assign metafields = array %}
{% assign metafields = metafields | push: metafield %}

{% assign variables = hash %}
{% assign variables["metafields"] = metafields %}

{% action "shopify" query: query, variables: variables %}
```
{% endtab %}
{% endtabs %}
