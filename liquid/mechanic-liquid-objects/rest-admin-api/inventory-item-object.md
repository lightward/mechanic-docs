# Inventory item object

## How to access it

* Use `{{ order.line_items[n].variant.inventory_item }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_item }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the InventoryItem resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/inventory/inventoryitem#properties)
* An array of related [inventory level objects](../inventory-level-object.md): `{{ variant.inventory_levels }}`
* The related [variant object](../variant-object.md): `{{ inventory_item.variant }}`

