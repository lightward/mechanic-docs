# 🚫 Line item object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.ks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:
{% endhint %}

## How to access it

* Access an array of line items using `{{ order.line_items }}` whenever [an order object](order.md) is available

## What it contains

* [Every property from Orders.line\_items in the Shopify REST Admin API](https://docs.usemechanic.com/article/400-the-line-item-object)
* The related [product object](product.md): `{{ line_item.product }}`&#x20;
* The related [variant object](variant.md): `{{ line_item.variant }}`&#x20;
* An array of properties, that also supports lookups by attribute name: `{% for prop in line_item.properties %}{% if prop.name == "Delivery window" %}{{ prop.value }}{% endif %}{% endfor %}`, or `{{ line_item.properties["Delivery window"] }}`&#x20;
