# 💪 Shop object

## How to access it

* Use `{{ shop }}` in any task, at any time

## What it contains

* [Every property from the Shop resource in the Shopify REST Admin API](https://shopify.dev/api/admin/rest/reference/store-properties/shop)

### Associated resources

{% hint style="warning" %}
Use caution when loading large sets of resources through the shop object. Using code like `{% product in shop.products %}` will result in Mechanic downloading the complete REST representation of all products in the store, which may be more data than is necessary or useful. When working with large amounts of data, consider [using GraphQL](../../../core/shopify/read/graphql-in-liquid.md) instead.



For clarity: looking up a single resource by ID will only result in a single REST API call, as in `{% assign product = shop.products[1234567890] %}`. If many of these requests are necessary, it may _still_ be useful to look to GraphQL, but this kind of usage does not load more than the specific, single resource identified.
{% endhint %}

* An index of [collection objects](collection.md)
  * `{{ shop.collections[1234567890] }}`
  * `{% for collection in shop.collections %}`
* An index of [product objects](product.md)
  * `{{ shop.products[1234567890] }}`
  * `{% for product in shop.products %}`
  * `{% for product in shop.products.published %}`
* An index of [variant objects](variant.md)
  * `{{ shop.variants[1234567890] }}`
  * `{% for variant in shop.variants %}`
* An index of [order objects](order.md)
  * `{{ shop.orders[1234567890] }}`
  * `{% for order in shop.orders %}`
  * `{% for order in shop.orders.paid %}`
* An index of [draft order objects](https://docs.usemechanic.com/article/386-the-draft-order-object)
  * `{{ shop.draft_orders[1234567890] }}`
  * `{% for draft_order in shop.draft_orders.invoice_sent %}`
* An index of [customer objects](customer-object.md)
  * `{{ shop.customers[1234567890] }}`
  * `{{ shop.customers["jdoe@example.com"] }}`
  * `{% for customer in shop.customers %}`
* An index of [price rule objects](price-rule.md)
  * `{{ shop.price_rules[1234567890] }}`
  * `{% for price_rule in shop.price_rules %}`
* A lookup of [discount code objects](discount-code.md)
  * `{{ shop.discount_codes["SUMMERTIME"] }}`
* An index of [blog objects](blog.md)
  * `{{ shop.blogs[1234567890] }}`
  * `{% for blog in shop.blogs %}`
* A set of [article tags](article.md)
  * `{{ shop.articles.tags }}`
* A set of [article authors](article.md)
  * `{{ shop.articles.authors }}`
* A set of [shipping zones](shipping-zone.md)
  * `{% for shipping_zone in shop.shipping_zones %}`
* The related [metafields object](metafields/metafield-collection.md)
  * `{{ shop.metafields }}`
