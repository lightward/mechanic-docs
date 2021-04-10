# Product object

## How to access it

* Use `{{ product }}` in tasks responding to shopify/products events
* Look up specific products by their ID, using `{{ shop.products[1234567890] }}`
* List published products using `{{ shop.products.published }}` 
* List unpublished products using `{{ shop.products.unpublished }}`  

## What it contains

* [Every property from the Product resource in the Shopify REST Admin AP](https://shopify.dev/docs/admin-api/rest/reference/products/product#properties)I **\(warning: Shopify delivers product.tags as a comma-delimited string,** _**not**_ **an array of strings!\)**
* An array of related [variant objects](variant-object.md): `{{ product.variants }}` 
* An array of related [collection objects](collection-object.md): `{{ product.collections }}`
* An array of related [product image objects](product-image-object.md): `{{ product.images }}`  
* The related [metafields object](metafields-object.md): `{{ product.metafields }}`

