# Inventory level object

## How to access it

* Use `{{ order.line_items[n].variant.inventory_levels[n] }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_levels[n] }}`  in tasks responding to shopify/products events

## What it contains

* [Every property from the InventoryLevel resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/inventory/inventorylevel#properties)
* The related [inventory item object](http://help.usemechanic.com/liquid/the-inventory-item-object): `{{ inventory_level.inventory_item }}`
* The related [variant object](variant.md): `{{ inventory_level.variant }}` 
* The related [location object](location.md): `{{ inventory_level.location }}` 

