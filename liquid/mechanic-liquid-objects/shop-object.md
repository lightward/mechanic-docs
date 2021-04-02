# Shop object

## How to access it

* Use `{{ shop }}`  in any task, at any time

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/store_properties/shop#properties)
* An index of [collection objects](https://docs.usemechanic.com/article/381-the-collection-object): `{{ shop.collections[1234567890] }}`, `{% for collection in shop.collections %}`, or use `shop.custom_collections` or `shop.smart_collections` to only return custom/smart collections
* An index of [product objects](https://docs.usemechanic.com/article/398-the-product-object): `{{ shop.products[1234567890] }}`, `{% for product in shop.products.published %}`
*  An index of [order objects](https://docs.usemechanic.com/article/401-the-order-object): `{{ shop.orders[1234567890] }}`, `{% for order in shop.orders.paid %}`
*  An index of [draft order objects](https://docs.usemechanic.com/article/386-the-draft-order-object): `{{ shop.draft_orders[1234567890] }}`, `{% for draft_order in shop.draft_orders.invoice_sent %}`
* An index of [customer objects](https://docs.usemechanic.com/article/399-the-customer-object): `{{ shop.customers[1234567890] }}`, `{{ shop.customers["jdoe@example.com"] }}`, `{% for customer in shop.customers %}` 
* An index of [price rule objects](https://docs.usemechanic.com/article/441-the-price-rule-object): `{{ shop.price_rules[1234567890] }}`, `{% for price_rule in shop.price_rules %}` 
* A lookup of [discount code objects](https://docs.usemechanic.com/article/442-the-discount-code-object): `{{ shop.discount_codes["SUMMERTIME"] }}`
* An index of [blog objects](https://docs.usemechanic.com/article/309-the-blog-object): `{{ shop.blogs[1234567890] }}`, `{% for blog in shop.blogs %}` 
* A set of [article](https://docs.usemechanic.com/article/308-the-article-object) tags: `{{ shop.articles.tags }}` 
* A set of [article](https://docs.usemechanic.com/article/308-the-article-object) authors: `{{ shop.articles.authors }}` 
* A set of [shipping zones](https://docs.usemechanic.com/article/462-the-shipping-zone-object): `{% for shipping_zone in shop.shipping_zones %}`
* The related [metafields object](https://docs.usemechanic.com/article/329-the-metafields-object): `{{ shop.metafields }}` 

