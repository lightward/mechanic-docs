# Metafield collection object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../../core/actions/shopify.md#graphql) going forward. The [product](../product.md) and [variant](../variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

{% hint style="info" %}
This page is part of a series: see [Metafields](./) for an overview on how Mechanic's Liquid implementation thinks about metafields.
{% endhint %}

A metafield collection object is an iterable set of metafields, which also supports using lookups to narrow the set by metafield namespace, or, further, to identify a single metafield by looking up by key.

## How to access it

* Use `resource.metafields` to retrieve the full set of that resource's metafields
* Use `resource.metafields.namespace` to retrieve the full set of that resource's metafields, filtering by metafield namespace
* Use `resource.metafields.namespace.key` to retrieve a [metafield representation object](metafield-representation-object.md) – _unless_ the referenced metafield has a [deprecated type](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields), in which case the parsed metafield value is returned directly

### Supported Liquid resources

* Article
* Blog
* Collection
* Customer
* Order
* Page
* Product
* Shop
* Variant
