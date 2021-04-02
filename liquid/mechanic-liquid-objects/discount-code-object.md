# Discount code object

 How to access it

* Look up specific discount codes by price rule ID and discount code ID, using `{{ shop.price_rules[12345].discount_codes[67890] }}`
* Look up a single discount code by its code, using `{{ shop.discount_codes["DISCOUNTABC123"] }}`
* Loop through all discount codes for a single: `{% for discount_code in price_rule.discount_codes %}`

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/discounts/discountcode)
* The related [price rule object](https://docs.usemechanic.com/article/441-the-price-rule-object): `{{ price_rule.discount_codes }}`

### Related articles

* [The shop object](https://docs.usemechanic.com/article/383-the-shop-object)
* [The price rule object](https://docs.usemechanic.com/article/441-the-price-rule-object)

