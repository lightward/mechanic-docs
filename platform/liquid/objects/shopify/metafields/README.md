# 🚫 Metafields

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](../product.md) and [variant](../variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

Mechanic's Liquid implementation contains three object types that address Shopify's metafield system:

* [Metafield](metafield-object.md) — Directly mirrors metafield data from the Shopify REST Admin API, including values that are possibly JSON-encoded strings.\

* [Metafield representation](metafield-representation-object.md) — Resembles [Shopify's metafield variable](https://shopify.dev/api/liquid/objects/metafield), containing the metafield type and the parsed, appropriately-typed value. Also contains a reference to the metafield object itself, and to any objects referenced by the metafield (i.e. to a product object, for a product\_reference metafield). Note that metafield representations are not available for or applicable to [deprecated metafields](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields).\

* [Metafield collection](metafield-collection.md) — An iterable set of metafields for a resource, which also supports narrowing by namespace, or resolving to an individual metafield by key.

To retrieve the value of a modern metafield, use `resource.metafields.namespace.key.value`.

To retrieve the value of a [deprecated](https://shopify.dev/api/liquid/objects/metafield#deprecated-metafields) metafield, use `resource.metafields.namespace.key`.
