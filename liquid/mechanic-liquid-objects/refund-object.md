# Refund object

## How to access it

* Use `{{ refund }}`  in tasks responding to shopify/refunds events
* Use `{% for refund in order.refunds %}`  in tasks responding to shopify/orders events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/orders/refund#properties)
* The related [order object](https://help.usemechanic.com/liquid/the-order-object): `{{ refund.order }` 
* An array of refund line items, each containing a [line item object](http://help.usemechanic.com/liquid/the-line-item-object): `{{ refund.refund_line_items.first.line_item }}`

