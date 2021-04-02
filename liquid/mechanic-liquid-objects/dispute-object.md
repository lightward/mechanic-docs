# Dispute object

##  How to access it

* Use `{{ dispute }}` in tasks responding to shopify/disputes events 

## What it contains

* [Every property from the Dispute resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/shopify_payments/dispute)
* The related [order object](order-object.md): `{{ dispute.order }}`

