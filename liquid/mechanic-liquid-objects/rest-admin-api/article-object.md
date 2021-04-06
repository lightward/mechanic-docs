# Article object

## How to access it

* Use `{% for article in blog.articles %}` 
* Look up specific articles by their ID, using `{{ shop.blogs[12345].articles[67890] }}`
* Use `{{ shop.articles.authors }}` for an array of the store's article authors
* Use `{{ shop.articles.tags }}` for a array of the store's article tags

## What it contains

* [Every property from the Article resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/online-store/article)
* The related [blog object](blog-object.md): `{{ article.blog }}`
* The related [metafields object](metafields-object.md): `{{ article.metafields }}`  

