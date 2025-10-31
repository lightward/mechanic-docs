# 🚫 Draft order object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Use `{{ draft_order }}`  in tasks responding to shopify/draft\_orders events
* Look up specific draft orders by their ID, using `{{ shop.draft_orders[12345678900] }}`

## What it contains

* [Every property from the DraftOrder resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/draftorder#properties)
* The related [order object](order.md), if any: `{{ draft_order.order }}`
* The related [customer object](customer-object.md), if any: `{{ draft_order.customer }}`&#x20;
* An array of [line item objects](line-item.md): `{{ draft_order.line_items }}`&#x20;
* An array of note\_attributes, that also supports lookups by attribute name: `{% for attr in draft_order.note_attributes %}{% if attr.name == "color" %}{{ attr.value }}{% endif %}{% endfor %}` , or `{{ draft_order.note_attributes.color }}`
