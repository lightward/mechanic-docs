# 🚫 Product image object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Use `{% for image in product.images %}` in tasks responding to shopify/products events, or any time you have [a product object](product.md)

## What it contains

* [Every property from the Product Image resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/products/product-image)
