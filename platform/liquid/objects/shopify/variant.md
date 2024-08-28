# 🚫 Variant object

{% hint style="danger" %}
**Shopify is deprecating some of the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read the deprecation notice** [**here**](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing)**.**&#x20;

Our recommendation is to use [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. It appears that Shopify will gradually phase out the REST API over time.

All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:
{% endhint %}

## How to access it

* Use `{{ order.line_items[n].variant }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n] }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the Product Variant resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/products/product-variant#properties)
* An array of related [inventory level objects](inventory-level.md): `{{ variant.inventory_levels }}`
* The related [inventory item object](inventory-item.md): `{{ variant.inventory_item }}`&#x20;
* The related [product object](product.md): `{{ variant.product }}`&#x20;
* The related [metafields object](metafields/metafield-collection.md): `{{ variant.metafields }}`&#x20;
