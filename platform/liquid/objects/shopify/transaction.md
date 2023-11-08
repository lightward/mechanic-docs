# Transaction object

## The transaction object

## How to access it

* Use `{% for transaction in order.transactions %}` in tasks responding to `shopify/orders` events
* Use `{% for transaction in refund.transactions %}` in tasks responding to `shopify/refunds` events

## What it contains

* [Every property from the Transaction resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/transaction#properties)

