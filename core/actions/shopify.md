# Shopify

The **Shopify** action sends requests to the [Shopify admin API](https://shopify.dev/docs/admin-api). It supports both REST and GraphQL requests.

{% hint style="danger" %}
**Important Notice**\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](shopify.md#graphql) going forward. The [product](../../platform/liquid/objects/shopify/product.md) and [variant](../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

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
To prepare complex query inputs, use the [graphql\_arguments](../../platform/liquid/filters/#graphql\_arguments) Liquid filter.
{% endhint %}

{% tabs %}
{% tab title="Liquid" %}
```liquid
{% raw %}
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
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": "\n  mutation {\n    customerCreate(\n      input: {\n        email: \"test@example.com\"\n      }\n    ) {\n      customer {\n        id\n      }\n      userErrors {\n        field\n        message\n      }\n    }\n  }\n"
  }
}
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
{% raw %}
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
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": {
      "query": "\n  mutation DeleteProduct($productId: ID!) {\n    productDelete(\n      input: {\n        id: $productId\n      }\n    ) {\n      userErrors {\n        field\n        message\n      }\n    }\n  }\n",
      "variables": {
        "productId": "gid://shopify/Product/1234567890"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Complex example

This example shows how the query and variables may be built up separately, and provided to the action using concise tag syntax.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% assign metafield_owner_id = "gid://shopify/Customer/507332001849" %}
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
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": {
      "query": "\n  mutation SetCustomerMetafield(\n    $customerId: ID!\n    $metafieldNamespace: String!\n    $metafieldKey: String!\n    $metafieldId: ID\n    $metafieldValue: String!\n  ) {\n    customerUpdate(\n      input: {\n        id: $customerId\n        metafields: [\n          {\n            id: $metafieldId\n            namespace: $metafieldNamespace\n            key: $metafieldKey\n            valueType: STRING\n            value: $metafieldValue\n          }\n        ]\n      }\n    ) {\n      userErrors {\n        field\n        message\n      }\n      customer {\n        metafield(\n          namespace: $metafieldNamespace\n          key: $metafieldKey\n        ){\n          id\n        }\n      }\n    }\n  }\n",
      "variables": {
        "customerId": "gid://shopify/Customer/700837494845",
        "metafieldNamespace": "test",
        "metafieldKey": "test",
        "metafieldId": "gid://shopify/Metafield/18788961353789",
        "metafieldValue": "1615244317"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### :warning: Resourceful REST

{% hint style="danger" %}
**Important Notice**\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](shopify.md#graphql) going forward. The [product](../../platform/liquid/objects/shopify/product.md) and [variant](../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.\

{% endhint %}

This usage style invokes the [Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest). It accepts an array of option values, containing these elements in order:

1. **Operation** Must be one of `"create"` , `"update"` , or `"delete"` .
2. **Resource specification** When creating, use a single string (e.g. `"customer"` ). When updating or deleting, use an array (e.g. `["customer", 123]` ).
3. **An object of attributes** Only applies to creating and updating.

#### Example: Creating a resource

This example creates a (minimal) customer record.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "create",
    "customer",
    {
      "email": "test@example.com"
    }
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "create",
      "customer",
      {
        "email": "test@example.com"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Example: Updating a resource

This example appends a line to the order note (assuming a task subscription to shopify/orders/create).

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "update",
    [
      "order",
      {{ order.id | json }}
    ],
    {
      "note": {{ order.note | append: newline | append: newline | append: "We're adding a note! 💪" | strip | json }}
    }
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "update",
      [
        "order",
        3656038711357
      ],
      {
        "note": "[customer-supplied note]\n\nWe're adding a note! 💪"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Example: Deleting a resource

This example deletes a product, having a certain ID.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "delete",
    ["product", 4814813560893]
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "delete",
      [
        "product",
        4814813560893
      ]
    ]
  }
}
```
{% endtab %}
{% endtabs %}

### :warning: Explicit REST

{% hint style="danger" %}
**Important Notice**\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](shopify.md#graphql) going forward. The [product](../../platform/liquid/objects/shopify/product.md) and [variant](../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

This usage style invokes [Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest). It accepts an array of option values, containing these elements in order:

1. **Operation** Must be one of `"get"`, `"post"` , `"put"` , or `"delete"`&#x20;
2. **Request path** The entire, literal request path to use, including the requested [API version](https://shopify.dev/concepts/about-apis/versioning#calling-an-api-version) — e.g. `"/admin/api/2020-01/orders.json"`
3. **A JSON object of attributes** In general, this means a wrapper object whose key is named after the current resource type, and whose value is the same set of data that would be used in the [resourceful](shopify.md#resourceful-rest) style

{% hint style="info" %}
When switching from resourceful to explicit REST, it's common to forget the outer wrapper object. This wrapper is required by Shopify for all request methods except GET and DELETE; it's handled automatically during resourceful usage, but must be handled manually during explicit usage.
{% endhint %}

#### Example: Creating a resource

This example creates a (minimal) customer record.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "post",
    "/admin/api/2020-01/customers.json",
    {
      "customer": {
        "email": "test@example.com"
      }
    }
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "post",
      "/admin/api/2020-01/customers.json",
      {
        "customer": {
          "email": "test@example.com"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Example: Updating a resource

This example appends a line to the order note (assuming a task subscription to shopify/orders/create).

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "put",
    "/admin/api/2020-01/orders/{{ order.id }}.json",
    {
      "order": {
        "note": {{ order.note | append: newline | append: newline | append: "We're adding a note! 💪" | strip | json }}
      }
    }
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "put",
      "/admin/api/2020-01/orders/3656063189053.json",
      {
        "order": {
          "note": "We're adding a note! 💪"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Example: Deleting a resource

This example deletes a product, having a certain ID.

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "shopify" %}
  [
    "delete",
    "/admin/api/2020-01/products/4814813724733.json"
  ]
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "shopify",
    "options": [
      "delete",
      "/admin/api/2020-01/products/4814813724733.json"
    ]
  }
}
```
{% endtab %}
{% endtabs %}
