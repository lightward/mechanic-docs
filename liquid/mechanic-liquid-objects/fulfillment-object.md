# Fulfillment object

## How to access it

* Use `{% for fulfillment in order.fulfillments %}` in tasks responding to shopify/orders events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/shipping-and-fulfillment/fulfillment)
* The related [order object](https://docs.usemechanic.com/article/401-the-order-object): `{{ fulfillment.order }}`
* The related [location object](https://docs.usemechanic.com/article/376-the-location-object): `{{ fulfillment.location }}`
* An array of [line item objects](https://docs.usemechanic.com/article/400-the-line-item-object): `{{ fulfillment.line_items }}`

