# 🚫 Product object

{% hint style="danger" %}
**Shopify is deprecating some of the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read the deprecation notice** [**here**](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing)**.**&#x20;

Our recommendation is to use [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. It appears that Shopify will gradually phase out the REST API over time.

All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:
{% endhint %}

## How to access it

* Use `{{ product }}` in tasks responding to shopify/products events
* Look up specific products by their ID, using `{{ shop.products[1234567890] }}`
* List published products using `{{ shop.products.published }}`&#x20;
* List unpublished products using `{{ shop.products.unpublished }}` &#x20;

## What it contains

* [Every property from the Product resource in the Shopify REST Admin AP](https://shopify.dev/docs/admin-api/rest/reference/products/product#properties)I **(warning: Shopify delivers product.tags as a comma-delimited string,** _**not**_ **an array of strings!)**
* An array of related [variant objects](variant.md): `{{ product.variants }}`&#x20;
* An array of related [collection objects](collection.md): `{{ product.collections }}`
* An array of related [product image objects](product-image.md): `{{ product.images }}` &#x20;
* The related [metafields object](metafields/metafield-collection.md): `{{ product.metafields }}`
