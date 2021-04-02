# Line item object

## How to access it

* Access an array of line items using `{{ order.line_items }}` whenever [an order object](order-object.md) is available

## What it contains

* [Every property from Orders.line\_items in the Shopify REST Admin API](https://docs.usemechanic.com/article/400-the-line-item-object)
* The related [product object](product-object.md): `{{ line_item.product }}` 
* The related [variant object](../variant-object.md): `{{ line_item.variant }}` 
* An array of properties, that also supports lookups by attribute name: `{% for prop in line_item.properties %}{% if prop.name == "Delivery window" %}{{ prop.value }}{% endif %}{% endfor %}`, or `{{ line_item.properties["Delivery window"] }}` 



