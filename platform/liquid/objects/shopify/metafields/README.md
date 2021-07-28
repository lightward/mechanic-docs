# Metafields

Mechanic's Liquid implementation contains three object types that address Shopify's metafield system:

* [Metafield](metafield-object.md) — describes a metafield resource from the Shopify REST Admin API. 
* [Metafield representation](metafield-representation-object.md) — mirrors [Shopify's Liquid implementation](https://shopify.dev/api/liquid/objects/metafield) for metafields, making the parsed metafield value and type available via `resource.metafields.namespace.key.type` and `resource.metafields.namespace.key.value`, in addition to a few Mechanic specific bonuses. This representation is not available for or applicable to [deprecated metafields](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields). 
* [Metafield collection](metafields.md) — an iterable set of metafields, available via `resource.metafields`, which may be narrowed by namespace \(i.e. `resource.metafields.namespace`\), or ultimately narrowed to a single object \(i.e. `resource.metafields.namespace.key`, which may be either a deprecated metafield or a metafield representation\).

