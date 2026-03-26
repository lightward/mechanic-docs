---
description: "How Mechanic Shopify automation tasks read and write store data — inline GraphQL queries for reads, and Shopify actions for mutations."
---

# Interacting with Shopify

Mechanic tasks have full access to the Shopify Admin API. Tasks can **read** data inline during execution — running GraphQL queries to fetch additional context beyond the triggering event — and **write** data via actions that run Shopify GraphQL mutations after the task completes. This gives tasks the same level of API access available to custom apps, without requiring you to build or host app infrastructure.

## Reading data

Use the [`shopify` Liquid filter](../../platform/liquid/filters/#shopify) to query Shopify data inline during a task run. This filter accepts a GraphQL query and returns the result immediately.

```liquid
{% capture query %}
  query {
    product(id: "gid://shopify/Product/1234567890") {
      title
      status
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{{ result.data.product.title }}
```

Learn more: [Reading data](read/)

## Writing data

Use the [Shopify action](../actions/shopify.md) to create, update, or delete Shopify resources. Shopify actions are queued during the task run and **performed after the task code finishes**, so you cannot use the result of a mutation in the same task run that creates it.

```liquid
{% action "shopify" %}
  mutation {
    productUpdate(input: { id: "gid://shopify/Product/1234567890", title: "New Title" }) {
      product {
        title
      }
      userErrors {
        field
        message
      }
    }
  }
{% endaction %}
```

Learn more: [Writing data](write.md)

{% hint style="warning" %}
Shopify REST is deprecated in Mechanic. Use the GraphQL Admin API for all new tasks. See [Converting tasks from Shopify REST to GraphQL](../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migration guidance.
{% endhint %}

## Responding to Shopify events

Mechanic can respond to Shopify webhooks (like `shopify/orders/create` or `shopify/products/update`) by subscribing tasks to the corresponding [event topics](../events/topics.md).

Learn more: [Responding to events](events/)

## Related

* [API rate limit](api-rate-limit.md) — how Mechanic manages Shopify's rate limits
* [API versions](api-versions.md) — how tasks select a Shopify API version
* [Shopify admin action links](admin-action-links.md)
