# 🚫 Article object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}



## How to access it

* Use `{% for article in blog.articles %}`&#x20;
* Look up specific articles by their ID, using `{{ shop.blogs[12345].articles[67890] }}`
* Use `{{ shop.articles.authors }}` for an array of the store's article authors
* Use `{{ shop.articles.tags }}` for a array of the store's article tags

## What it contains

* [Every property from the Article resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/online-store/article)
* The related [blog object](blog.md): `{{ article.blog }}`
* The related [metafields object](metafields/metafield-collection.md): `{{ article.metafields }}` &#x20;
