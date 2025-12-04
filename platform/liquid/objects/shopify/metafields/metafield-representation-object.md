# Metafield representation object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../../core/actions/shopify.md#graphql) going forward. The [product](../product.md) and [variant](../variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

{% hint style="info" %}
This page is part of a series: see [Metafields](./) for an overview on how Mechanic's Liquid implementation thinks about metafields.
{% endhint %}

Mechanic's metafield representation object mirrors [Shopify's metafield Liquid object](https://shopify.dev/api/liquid/objects/metafield), in that it makes the metafield type and value easily available, in a usable form. It is not the same as Mechanic's [metafield object](metafield-object.md), which contains the REST Admin API representation of a metafield.

{% hint style="warning" %}
Metafield representation objects are only available for modern metafield types. They are not available for [deprecated metafields](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields) (i.e. json\_string, number, and string).
{% endhint %}

## What it contains

A metafield representation object always contains these three properties:

* `type` — the type of the metafield (see [Shopify's reference list](https://shopify.dev/apps/metafields/definitions/types))
* `value` — the parsed, appropriately-typed value of the metafield
* `metafield` — the source [metafield object](metafield-object.md), useful for retrieving the metafield ID

### Resource references

For reference types that map to a REST API resource (e.g. page\_reference, product\_reference, and variant\_reference), a metafield representation object _also_ contains a property named after the resource in question.

For example, a product\_reference metafield representation object contains a `product` property, which holds the associated [product object](../product.md). This means that the referenced product may be retrieved using `resource.metafields.namespace.key.product`.

## How to access it

A metafield representation object can only be retrieved via [metafield collection](metafield-collection.md) lookup: `resource.metafields.namespace.key`.
