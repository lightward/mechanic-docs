# Metafield object

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



The metafield object reflects the REST Admin API's representation of a metafield.

Mechanic includes support for the metafield object for completeness, but it _is not_ the most useful way to go about using metafield values (see [Metafield representation object](metafield-representation-object.md)).

## What it contains

* Every property from [the Metafield resource in the Shopify REST Admin API](https://shopify.dev/api/admin/rest/reference/metafield)

{% hint style="warning" %}
For many metafield types (e.g. dimension, weight, decimal, etc), the `value` attribute of the metafield object is a JSON-encoded string. For most scalar types (e.g. boolean, number\_integer), the `value` attribute has a matching type.

The simplest way to access a usable version of a metafield value is via the [metafield representation object](metafield-representation-object.md), i.e. `resource.metafields.namespace.key.value`.
{% endhint %}

## How to access it

* For modern metafields, use `resource.metafields.namespace.key.metafield` to retrieve the metafield object itself.
  * Use `resource.metafields.namespace.key` to access the [metafield representation object](metafield-representation-object.md) for that metafield.
* For deprecated metafields, use `resource.metafields.namespace.key` to retrieve the parsed, appropriately-typed metafield value.
