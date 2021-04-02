# Order risk object

## How to access it

* Use `{% for risk in order.risks %}` in tasks responding to `shopify/orders`  events, or in any other scenario with an [order object](order-object.md)
* Use `{{ order.risks[12345].message }}` to retrieve a specific order risk, given an order object

## What it contains

* [Every property from the Order Risk resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/order-risk#properties)

