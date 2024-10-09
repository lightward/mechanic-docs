# 🚫 Discount code object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API competely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Look up specific discount codes by price rule ID and discount code ID, using `{{ shop.price_rules[12345].discount_codes[67890] }}`
* Look up a single discount code by its code, using `{{ shop.discount_codes["DISCOUNTABC123"] }}`
* Loop through all discount codes for a single: `{% for discount_code in price_rule.discount_codes %}`

## What it contains

* [Every property from the DiscountCode resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/discounts/discountcode)
* The related [price rule object](price-rule.md): `{{ price_rule.discount_codes }}`

### Related articles

* [The shop object](shop.md)
* [The price rule object](price-rule.md)
