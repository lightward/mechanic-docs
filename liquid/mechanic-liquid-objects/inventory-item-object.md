# Inventory item object

## How to access it

* Use `{{ order.line_items[n].variant.inventory_item }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_item }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/inventory/inventoryitem#properties)
* An array of related [inventory level objects](http://help.usemechanic.com/liquid/the-inventory-level-object): `{{ variant.inventory_levels }}`
* The related [variant object](http://help.usemechanic.com/liquid/the-variant-object): `{{ inventory_item.variant }}`

