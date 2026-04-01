# 🚫 Inventory item object

{% hint style="danger" %}
**Important Notice**

\
Shopify has deprecated the REST Admin API that these Mechanic objects depend on. The product and variant endpoints were removed on Feb 1, 2025, and Shopify is phasing out the remaining REST endpoints over time. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects ceased to work on Feb 1, 2025 due to changes made by Shopify. Shopify is phasing out the REST API entirely — read more [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Use `{{ order.line_items[n].variant.inventory_item }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_item }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the InventoryItem resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/inventory/inventoryitem#properties)
* An array of related [inventory level objects](inventory-level.md): `{{ variant.inventory_levels }}`
* The related [variant object](variant.md): `{{ inventory_item.variant }}`
