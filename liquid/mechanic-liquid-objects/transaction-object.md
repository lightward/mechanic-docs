# Transaction object

## The transaction object

## How to access it

* Use `{% for transaction in order.transactions %}` in tasks responding to `shopify/orders` events
* Use `{% for transaction in refund.transactions %}` in tasks responding to `shopify/refunds` events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/orders/transaction#properties)

