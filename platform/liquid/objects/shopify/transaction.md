# 🚫 Transaction object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## The transaction object

## How to access it

* Use `{% for transaction in order.transactions %}` in tasks responding to `shopify/orders` events
* Use `{% for transaction in refund.transactions %}` in tasks responding to `shopify/refunds` events

## What it contains

* [Every property from the Transaction resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/transaction#properties)
