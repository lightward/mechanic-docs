# 🚫 Draft order object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
