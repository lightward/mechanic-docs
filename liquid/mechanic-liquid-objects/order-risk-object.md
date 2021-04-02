# Order risk object

## How to access it

* Use `{% for risk in order.risks %}` in tasks responding to `shopify/orders`  events, or in any other scenario with an [order object](https://help.usemechanic.com/liquid/the-order-object)
* Use `{{ order.risks[12345].message }}` to retrieve a specific order risk, given an order object

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/orders/order-risk#properties)

