# Dispute object

##  How to access it

* Use `{{ dispute }}` in tasks responding to shopify/disputes events 

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/shopify_payments/dispute)
* The related [order object](https://docs.usemechanic.com/article/401-the-order-object): `{{ dispute.order }}`

