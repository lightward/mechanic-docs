# Fulfillment order object

## How to access it

* Use `{% for fulfillment_order in order.fulfillment_orders %}` in tasks responding to shopify/orders events

## What it contains

* [Every property from the Fulfillment resource in the Shopify REST Admin API](https://shopify.dev/docs/api/admin-rest/2023-10/resources/fulfillmentorder#resource-object)
* The related [order object](order.md): `{{ fulfillment_order.order }}`
