# Order object

## How to access it

* Use `{{ order }}` in tasks responding to shopify/orders events
* Use `{{ refund.order }}` in tasks responding to shopify/refunds events
* Look up specific orders by their ID, using `{{ shop.orders[12345678900] }}` 
* Loop through all open orders: `{% for order in shop.orders %}`

Or, loop through all orders, not just open orders:  
\`

\`

Or, use these sub-objects to loop through certain subsets of orders:  
`shop.orders.open`  
`shop.orders.closed`  
`shop.orders.cancelled`  
`shop.orders.authorized`  
`shop.orders.pending`  
`shop.orders.paid`  
`shop.orders.partially_paid`  
`shop.orders.refunded`  
`shop.orders.voided`  
`shop.orders.partially_refunded`  
`shop.orders.unpaid`  
`shop.orders.shipped`  
`shop.orders.partial`  
`shop.orders.unshipped`

Or, combine to be even more selective:  
`shop.orders.any.paid.unshipped`  
`shop.orders.refunded.shipped`  
`shop.orders.open.pending.unshipped`

## What it contains

* [Every property from the Order resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/orders/order#properties) **\(warning: Shopify delivers order.tags as a comma-delimited string,** _**not**_ **an array of strings!\)**
* The related [customer object](customer-object.md): `{{ order.customer }}`
* An array of [line item objects](line-item-object.md): `{{ order.line_items }}` 
* An array of [refund objects](refund-object.md): `{{ order.refunds }}` 
* An array of [order risk objects](order-risk-object.md): `{{ order.risks }}` 
* An array of [transaction objects](transaction-object.md): `{{ order.transactions }}`
* An array of [fulfillment objects](fulfillment-object.md): `{{ order.fulfillments }}` 
* An array of note attributes, that also supports lookups by attribute name: `{% for attr in order.note_attributes %}{% if attr.name == "color" %}{{ attr.value }}{% endif %}{% endfor %}`, or `{{ order.note_attributes.color }}` 

## Notes

Out of the box, only orders from the last 60 days are accessible. To give Mechanic access to your complete order history, [enable "read all orders"](https://help.usemechanic.com/tutorials/enabling-read_all_orders).

