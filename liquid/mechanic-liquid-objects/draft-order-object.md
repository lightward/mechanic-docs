# Draft order object

## How to access it

* Use `{{ draft_order }}`  in tasks responding to shopify/draft\_orders events
* Look up specific draft orders by their ID, using `{{ shop.draft_orders[12345678900] }}`

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/orders/draftorder#properties)
* The related [order object](http://help.usemechanic.com/liquid/the-order-object), if any: `{{ draft_order.order }}`
* The related [customer object](http://help.usemechanic.com/liquid/the-customer-object), if any: `{{ draft_order.customer }}` 
* An array of [line item objects](http://help.usemechanic.com/liquid/the-line-item-object): `{{ draft_order.line_items }}` 
* An array of note\_attributes, that also supports lookups by attribute name: `{% for attr in draft_order.note_attributes %}{% if attr.name == "color" %}{{ attr.value }}{% endif %}{% endfor %}` , or `{{ draft_order.note_attributes.color }}`

