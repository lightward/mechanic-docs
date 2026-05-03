# Metafield collection object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](../README.md) and [migration guides](../../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
