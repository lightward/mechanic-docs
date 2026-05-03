# 🚫 Product object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
