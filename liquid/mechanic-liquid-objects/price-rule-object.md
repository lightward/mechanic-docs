# Price rule object

##  How to access it

* Look up specific price rules by ID, using `{{ shop.price_rules[12345] }}`
* Loop through all price rules: `{% for price_rule in shop.price_rules %}`

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/discounts/pricerule)
* An index of related [discount code objects](https://docs.usemechanic.com/article/442-the-discount-code-object): `{{ price_rule.discount_codes }}`

Did this answer your question?  Thanks for the feedback There was a problem submitting your feedback. Please try again later. Last updated on March 25, 2020

### Related articles

* [The shop object](https://docs.usemechanic.com/article/383-the-shop-object)
* [The discount code object](https://docs.usemechanic.com/article/442-the-discount-code-object)

