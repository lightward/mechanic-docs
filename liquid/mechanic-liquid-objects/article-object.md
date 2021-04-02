# Article object

## How to access it

* Use `{% for article in blog.articles %}` 
* Look up specific articles by their ID, using `{{ shop.blogs[12345].articles[67890] }}`
* Use `{{ shop.articles.authors }}` for an array of the store's article authors
* Use `{{ shop.articles.tags }}` for a array of the store's article tags

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/online-store/article)
* The related [blog object](https://help.usemechanic.com/en/articles/3637716-the-blog-object): `{{ article.blog }}`
* The related [metafields object](https://help.usemechanic.com/en/articles/3347736-the-metafields-object): `{{ article.metafields }}`  

