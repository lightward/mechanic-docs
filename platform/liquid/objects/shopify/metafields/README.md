# 🚫 Metafields

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](../README.md) and [migration guides](../../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

Mechanic's Liquid implementation contains three object types that address Shopify's metafield system:

* [Metafield](metafield-object.md) — Directly mirrors metafield data from the Shopify REST Admin API, including values that are possibly JSON-encoded strings.\

* [Metafield representation](metafield-representation-object.md) — Resembles [Shopify's metafield variable](https://shopify.dev/api/liquid/objects/metafield), containing the metafield type and the parsed, appropriately-typed value. Also contains a reference to the metafield object itself, and to any objects referenced by the metafield (i.e. to a product object, for a product\_reference metafield). Note that metafield representations are not available for or applicable to [deprecated metafields](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields).\

* [Metafield collection](metafield-collection.md) — An iterable set of metafields for a resource, which also supports narrowing by namespace, or resolving to an individual metafield by key.

To retrieve the value of a modern metafield, use `resource.metafields.namespace.key.value`.

To retrieve the value of a [deprecated](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields) metafield, use `resource.metafields.namespace.key`.
