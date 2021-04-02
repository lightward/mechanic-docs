# Blog object

## How to access it

* Use `{% for blog in shop.blogs %}`
* Look up specific blogs by their ID, using `{{ shop.blogs[1234567890] }}` 

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/online-store/blog)
* An array of related [article objects](https://help.usemechanic.com/en/articles/3637718-the-article-object): `{{ blog.articles }}` 
* The related [metafields object](https://help.usemechanic.com/en/articles/3347736-the-metafields-object): `{{ blog.metafields }}` 

