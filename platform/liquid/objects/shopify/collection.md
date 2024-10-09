# 🚫 Collection object

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

* Use `{{ collection }}`  in tasks responding to shopify/collections events
* Look up specific collections by their ID, using `{{ shop.collections[1234567890] }}`
* Locate it in the array of product collections, using `{{ products.collections[0] }}`, in tasks responding to shopify/products events

## What it contains

* Every property from the Collection resource in the Shopify REST Admin API — see documentation for [custom collections](https://shopify.dev/docs/admin-api/rest/reference/products/customcollection), and for [automatic/smart collections](https://shopify.dev/docs/admin-api/rest/reference/products/smartcollection)
* An array of related [product objects](product.md), ordered by their position in the collection: `{{ collection.products }}`&#x20;
