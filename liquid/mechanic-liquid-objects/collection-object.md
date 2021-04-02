# Collection object

## How to access it

* Use `{{ collection }}`  in tasks responding to shopify/collections events
* Look up specific collections by their ID, using `{{ shop.collections[1234567890] }}`
* Locate it in the array of product collections, using `{{ products.collections[0] }}`, in tasks responding to shopify/products events

## What it contains

* Every property from the Shopify API — see documentation for [custom collections](https://help.shopify.com/en/api/reference/products/customcollection), and for [automatic/smart collections](https://help.shopify.com/en/api/reference/products/smartcollection)
* An array of related [product objects](https://help.usemechanic.com/liquid/the-product-object), ordered by their position in the collection: `{{ collection.products }}` 

