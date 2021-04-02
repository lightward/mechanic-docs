# Fulfillment\_event object

##  How to access it

* Use `{{ fulfillment_event }}` in tasks responding to shopify/fulfillment\_events events

## What it contains

* [Every property from the FulfillmentEvent resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/shipping-and-fulfillment/fulfillmentevent)
* The related [order object](../order-object.md): `{{ fulfillment_event.order }}`
* The related [fulfillment object](fulfillment-object.md): `{{ fulfillment_event.fulfillment }}`

