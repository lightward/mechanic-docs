# Variant object

## How to access it

* Use `{{ order.line_items[n].variant }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n] }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the Product Variant resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/products/product-variant#properties)
* An array of related [inventory level objects](inventory-level-object.md): `{{ variant.inventory_levels }}`
* The related [inventory item object](inventory-item-object.md): `{{ variant.inventory_item }}` 
* The related [product object](product-object.md): `{{ variant.product }}` 
* The related [metafields object](metafields-object.md): `{{ variant.metafields }}` 

