# 🚫 Shop object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Use `{{ shop }}` in any task, at any time

## What it contains

* [Every property from the Shop resource in the Shopify REST Admin API](https://shopify.dev/api/admin/rest/reference/store-properties/shop)

### Associated resources

{% hint style="warning" %}
Use caution when loading large sets of resources through the shop object. Using code like `{% product in shop.products %}` will result in Mechanic downloading the complete REST representation of all products in the store, which may be more data than is necessary or useful. When working with large amounts of data, consider \[using GraphQL]\(../../../../core/shopify/read/graphql-in-liquid.md) instead.

For clarity: looking up a single resource by ID will only result in a single REST API call, as in `{% assign product = shop.products[1234567890] %}`

. If many of these requests are necessary, it may _still_ be useful to look to GraphQL, but this kind of usage does not load more than the specific, single resource identified.
{% endhint %}

* The admin URL of the shop (e.g. https://admin.shopify.com/store/mechanic-shop/)
  * `{{ shop.admin_url }}`
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
