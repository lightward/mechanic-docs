# Product object

## How to access it

* Use `{{ product }}` in tasks responding to shopify/products events
* Look up specific products by their ID, using `{{ shop.products[1234567890] }}`
* List published products using `{{ shop.products.published }}` 
* List unpublished products using `{{ shop.products.unpublished }}`  

## What it contains

* [Every property from the Shopify REST API](https://help.shopify.com/en/api/reference/products/product#properties) \(warning: Shopify delivers product.tags as a comma-delimited string, _not_ an array of strings!\)
* An array of related [variant objects](http://help.usemechanic.com/liquid/the-variant-object): `{{ product.variants }}` 
* An array of related [collection objects](https://help.usemechanic.com/liquid/the-collection-object): `{{ product.collections }}`
* An array of related [product image objects](https://help.usemechanic.com/en/articles/3294400-the-product-image-object): `{{ product.images }}`  
* The related [metafields object](http://help.usemechanic.com/en/articles/3347736-the-metafields-object): `{{ product.metafields }}`

