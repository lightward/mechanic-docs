# Fulfillment\_event object

##  How to access it

* Use `{{ fulfillment_event }}` in tasks responding to shopify/fulfillment\_events events

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/shipping-and-fulfillment/fulfillmentevent)
* The related [order object](https://docs.usemechanic.com/article/401-the-order-object): `{{ fulfillment_event.order }}`
* The related [fulfillment object](https://docs.usemechanic.com/article/315-the-fulfillment-object): `{{ fulfillment_event.fulfillment }}`

