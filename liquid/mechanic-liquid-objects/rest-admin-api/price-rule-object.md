# Price rule object

##  How to access it

* Look up specific price rules by ID, using `{{ shop.price_rules[12345] }}`
* Loop through all price rules: `{% for price_rule in shop.price_rules %}`

## What it contains

* [Every property from PriceRule resource in the Shopify REST Admin API](https://shopify.dev/docs/admin-api/rest/reference/discounts/pricerule)
* An index of related [discount code objects](https://shopify.dev/docs/admin-api/rest/reference/discounts/pricerule): `{{ price_rule.discount_codes }}`

### Related articles

* [The shop object](shop-object.md)
* [The discount code object](discount-code-object.md)



