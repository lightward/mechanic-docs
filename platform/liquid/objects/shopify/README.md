# ⚠️ Shopify REST Admin API

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating some of the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read the deprecation notice [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing). \
\
Our recommendation is to use [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. It appears that Shopify will gradually phase out the REST API over time.

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:
{% endhint %}
