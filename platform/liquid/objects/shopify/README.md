# ⚠️ Shopify REST Admin API

{% hint style="danger" %}
**Important Notice**

Shopify has deprecated the REST Admin API that these Mechanic objects depend on. The product and variant endpoints were removed on Feb 1, 2025, and Shopify is phasing out the remaining REST endpoints over time. Read more [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).

Use [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects no longer work. The remaining REST objects will follow as Shopify continues the migration.

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}
