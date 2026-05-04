# 🚫 Inventory level object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Use `{{ order.line_items[n].variant.inventory_levels[n] }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_levels[n] }}`  in tasks responding to shopify/products events

## What it contains

* [Every property from the InventoryLevel resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/inventory/inventorylevel#properties)
* The related [inventory item object](inventory-item.md): `{{ inventory_level.inventory_item }}`
* The related [variant object](variant.md): `{{ inventory_level.variant }}`&#x20;
* The related [location object](location.md): `{{ inventory_level.location }}`&#x20;
