# 🚫 Refund object

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

* Use `{{ refund }}`  in tasks responding to shopify/refunds events
* Use `{% for refund in order.refunds %}`  in tasks responding to shopify/orders events

## What it contains

* [Every property from the Refund resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/refund#properties)
* The related [order object](order.md): `{{ refund.order }`&#x20;
* An array of refund line items, each containing a [line item object](line-item.md): `{{ refund.refund_line_items.first.line_item }}`
