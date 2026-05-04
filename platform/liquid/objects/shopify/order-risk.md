# 🚫 Order risk object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Use `{% for risk in order.risks %}` in tasks responding to `shopify/orders`  events, or in any other scenario with an [order object](order.md)
* Use `{{ order.risks[12345].message }}` to retrieve a specific order risk, given an order object

## What it contains

* [Every property from the Order Risk resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/order-risk#properties)
