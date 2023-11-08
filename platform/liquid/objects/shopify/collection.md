# Collection object

## How to access it

* Use `{{ collection }}`  in tasks responding to shopify/collections events
* Look up specific collections by their ID, using `{{ shop.collections[1234567890] }}`
* Locate it in the array of product collections, using `{{ products.collections[0] }}`, in tasks responding to shopify/products events

## What it contains

* Every property from the Collection resource in the Shopify REST Admin API — see documentation for [custom collections](https://shopify.dev/docs/admin-api/rest/reference/products/customcollection), and for [automatic/smart collections](https://shopify.dev/docs/admin-api/rest/reference/products/smartcollection)
* An array of related [product objects](product.md), ordered by their position in the collection: `{{ collection.products }}` 

