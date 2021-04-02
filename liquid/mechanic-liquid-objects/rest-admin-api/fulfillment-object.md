# Fulfillment object

## How to access it

* Use `{% for fulfillment in order.fulfillments %}` in tasks responding to shopify/orders events

## What it contains

* [Every property from the Fulfillment resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/shipping-and-fulfillment/fulfillment)
* The related [order object](../order-object.md): `{{ fulfillment.order }}`
* The related [location object](../location-object.md): `{{ fulfillment.location }}`
* An array of [line item objects](../line-item-object.md): `{{ fulfillment.line_items }}`

