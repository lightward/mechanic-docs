# 🚫 Price rule object

{% hint style="danger" %}
**Important Notice**

\
Shopify is deprecating the Shopify Admin REST API which the Mechanic REST objects depend on. The first round of deprecations involve the product and variant endpoints. Read about the deprecation  [here](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing) and [here](https://shopify.dev/docs/apps/build/graphql/migrate).\
\
Use the [GraphQL](../../../../core/actions/integrations/shopify.md#graphql) going forward. The [product](product.md) and [variant](variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. Shopify will phase out the REST API completely over time, you can read more about this [here](https://shopify.dev/docs/apps/build/graphql/migrate).

\
All of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which will provide a model for how you can update your custom tasks. You'll be able to update your non-customized library tasks with a click of a button :relaxed:\
\
Please see these [guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/) for migrating your custom tasks to GraphQL.
{% endhint %}

## How to access it

* Look up specific price rules by ID, using `{{ shop.price_rules[12345] }}`
* Loop through all price rules: `{% for price_rule in shop.price_rules %}`

## What it contains

* [Every property from PriceRule resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/discounts/pricerule)
* An index of related [discount code objects](https://shopify.dev/docs/admin-api/rest/reference/discounts/pricerule): `{{ price_rule.discount_codes }}`

### Related articles

* [The shop object](shop.md)
* [The discount code object](discount-code.md)
