# Draft order object

## How to access it

* Use `{{ draft_order }}`  in tasks responding to shopify/draft\_orders events
* Look up specific draft orders by their ID, using `{{ shop.draft_orders[12345678900] }}`

## What it contains

* [Every property from the DraftOrder resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/draftorder#properties)
* The related [order object](order-object.md), if any: `{{ draft_order.order }}`
* The related [customer object](customer-object.md), if any: `{{ draft_order.customer }}` 
* An array of [line item objects](line-item-object.md): `{{ draft_order.line_items }}` 
* An array of note\_attributes, that also supports lookups by attribute name: `{% for attr in draft_order.note_attributes %}{% if attr.name == "color" %}{{ attr.value }}{% endif %}{% endfor %}` , or `{{ draft_order.note_attributes.color }}`



