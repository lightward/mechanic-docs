# 🚫 Price rule object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
