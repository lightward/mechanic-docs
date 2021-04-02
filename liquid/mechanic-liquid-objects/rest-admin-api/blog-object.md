# Blog object

## How to access it

* Use `{% for blog in shop.blogs %}`
* Look up specific blogs by their ID, using `{{ shop.blogs[1234567890] }}` 

## What it contains

* [Every property from the Blog resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/online-store/blog)
* An array of related [article objects](article-object.md): `{{ blog.articles }}` 
* The related [metafields object](../metafields-object.md): `{{ blog.metafields }}` 



