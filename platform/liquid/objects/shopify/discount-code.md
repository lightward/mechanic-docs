# 🚫 Discount code object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
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
