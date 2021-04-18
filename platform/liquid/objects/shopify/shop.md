# 💪 Shop object

## How to access it

* Use `{{ shop }}`  in any task, at any time

## What it contains

* [Every property from the Shop resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/store-properties/shop#properties)
* An index of [collection objects](collection.md): `{{ shop.collections[1234567890] }}`, `{% for collection in shop.collections %}`, or use `shop.custom_collections` or `shop.smart_collections` to only return custom/smart collections
* An index of [product objects](product.md): `{{ shop.products[1234567890] }}`, `{% for product in shop.products.published %}`
* An index of [order objects](order.md): `{{ shop.orders[1234567890] }}`, `{% for order in shop.orders.paid %}`
* An index of [draft order objects](https://docs.usemechanic.com/article/386-the-draft-order-object): `{{ shop.draft_orders[1234567890] }}`, `{% for draft_order in shop.draft_orders.invoice_sent %}`
* An index of [customer objects](customer-object.md): `{{ shop.customers[1234567890] }}`, `{{ shop.customers["jdoe@example.com"] }}`, `{% for customer in shop.customers %}` 
* An index of [price rule objects](price-rule.md): `{{ shop.price_rules[1234567890] }}`, `{% for price_rule in shop.price_rules %}` 
* A lookup of [discount code objects](discount-code.md): `{{ shop.discount_codes["SUMMERTIME"] }}`
* An index of [blog objects](blog.md): `{{ shop.blogs[1234567890] }}`, `{% for blog in shop.blogs %}` 
* A set of [article](article.md) tags: `{{ shop.articles.tags }}` 
* A set of [article](article.md) authors: `{{ shop.articles.authors }}` 
* A set of [shipping zones](shipping-zone.md): `{% for shipping_zone in shop.shipping_zones %}`
* The related [metafields object](metafields.md): `{{ shop.metafields }}` 

