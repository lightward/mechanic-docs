# Inventory level object

## How to access it

* Use `{{ order.line_items[n].variant.inventory_levels[n] }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n].inventory_levels[n] }}`  in tasks responding to shopify/products events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/inventory/inventorylevel#properties)
* The related [inventory item object](http://help.usemechanic.com/liquid/the-inventory-item-object): `{{ inventory_level.inventory_item }}`
* The related [variant object](http://help.usemechanic.com/liquid/the-variant-object): `{{ inventory_level.variant }}` 
* The related [location object](http://help.usemechanic.com/liquid/the-location-object): `{{ inventory_level.location }}` 

