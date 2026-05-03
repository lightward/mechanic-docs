# 🚫 Shipping zone object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

### How to access it

* Use `{% for shipping_zone in shop.shipping_zones %}`&#x20;

## What it contains

* [Every property from the ShippingZone resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/store-properties/shippingzone)
