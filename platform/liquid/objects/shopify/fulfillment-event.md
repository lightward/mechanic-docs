# 🚫 Fulfillment event object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Use `{{ fulfillment_event }}` in tasks responding to shopify/fulfillment\_events events

## What it contains

* [Every property from the FulfillmentEvent resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/shipping-and-fulfillment/fulfillmentevent)
* The related [order object](order.md): `{{ fulfillment_event.order }}`
* The related [fulfillment object](fulfillment.md): `{{ fulfillment_event.fulfillment }}`
