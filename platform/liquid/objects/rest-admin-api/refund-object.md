# Refund object

## How to access it

* Use `{{ refund }}`  in tasks responding to shopify/refunds events
* Use `{% for refund in order.refunds %}`  in tasks responding to shopify/orders events

## What it contains

* [Every property from the Refund resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/refund#properties)
* The related [order object](order-object.md): `{{ refund.order }` 
* An array of refund line items, each containing a [line item object](line-item-object.md): `{{ refund.refund_line_items.first.line_item }}`

