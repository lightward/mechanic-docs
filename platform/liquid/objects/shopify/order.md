# 🚫 Order object

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

* Use `{{ order }}` in tasks responding to shopify/orders events
* Use `{{ refund.order }}` in tasks responding to shopify/refunds events
* Look up specific orders by their ID, using `{{ shop.orders[12345678900] }}`&#x20;
* Loop through all _**open**_ orders: `{% for order in shop.orders %}`

Or, loop through _**all**_ orders, not just open orders: `{% for order in shop.orders.any %}`

Or, use these sub-objects to loop through certain subsets of orders:\
`shop.orders.open`\
`shop.orders.closed`\
`shop.orders.cancelled`\
`shop.orders.authorized`\
`shop.orders.pending`\
`shop.orders.paid`\
`shop.orders.partially_paid`\
`shop.orders.refunded`\
`shop.orders.voided`\
`shop.orders.partially_refunded`\
`shop.orders.unpaid`\
`shop.orders.shipped`\
`shop.orders.partial`\
`shop.orders.unshipped`

Or, combine to be even more selective:\
`shop.orders.any.paid.unshipped`\
`shop.orders.refunded.shipped`\
`shop.orders.open.pending.unshipped`

## What it contains

* [Every property from the Order resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/order#properties) **(warning: Shopify delivers order.tags as a comma-delimited string,** _**not**_ **an array of strings!)**
* The related [customer object](customer-object.md): `{{ order.customer }}`
* An array of [line item objects](line-item.md): `{{ order.line_items }}`&#x20;
* An array of [refund objects](refund.md): `{{ order.refunds }}`&#x20;
* An array of [order risk objects](order-risk.md): `{{ order.risks }}`&#x20;
* An array of [transaction objects](transaction.md): `{{ order.transactions }}`
* An array of [fulfillment objects](fulfillment.md): `{{ order.fulfillments }}`&#x20;
* An array of [fulfillment orders objects](fulfillment-order.md): `{{ order.fulfillment_orders }}`&#x20;
* An array of note attributes, that also supports lookups by attribute name: `{% for attr in order.note_attributes %}{% if attr.name == "color" %}{{ attr.value }}{% endif %}{% endfor %}`, or `{{ order.note_attributes.color }}`&#x20;

## Notes

Out of the box, only orders from the last 60 days are accessible. To give Mechanic access to your complete order history, [enable "read all orders"](https://help.usemechanic.com/tutorials/enabling-read_all_orders).
