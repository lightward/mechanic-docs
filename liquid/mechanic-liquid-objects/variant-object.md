# Variant object

## How to access it

* Use `{{ order.line_items[n].variant }}` in tasks responding to shopify/orders events
* Use `{{ product.variants[n] }}` in tasks responding to shopify/products events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/products/product_variant#properties)
* An array of related [inventory level objects](http://help.usemechanic.com/liquid/the-inventory-level-object): `{{ variant.inventory_levels }}`
* The related [inventory item object](http://help.usemechanic.com/liquid/the-inventory-item-object): `{{ variant.inventory_item }}` 
* The related [product object](http://help.usemechanic.com/liquid/the-product-object): `{{ variant.product }}` 
* The related [metafields object](http://help.usemechanic.com/en/articles/3347736-the-metafields-object): `{{ variant.metafields }}` 

Did this answer your question?  Thanks for the feedback There was a problem submitting your feedback. Please try again later. Last updated on January 22, 2020

