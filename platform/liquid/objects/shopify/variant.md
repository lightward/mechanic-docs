# 🚫 Variant object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
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
