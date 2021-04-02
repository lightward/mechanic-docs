# Line Item object

## How to access it

* Access an array of line items using `{{ order.line_items }}` whenever [an order object](http://help.usemechanic.com/liquid/the-order-object) is available

## What it contains

* [Every property from the Shopify API](https://shopify.dev/docs/admin-api/rest/reference/orders/order#line-items-property-2020-04)
* The related [product object](http://help.usemechanic.com/liquid/the-product-object): `{{ line_item.product }}` 
* The related [variant object](http://help.usemechanic.com/liquid/the-variant-object): `{{ line_item.variant }}` 
* An array of properties, that also supports lookups by attribute name: `{% for prop in line_item.properties %}{% if prop.name == "Delivery window" %}{{ prop.value }}{% endif %}{% endfor %}`, or `{{ line_item.properties["Delivery window"] }}` 

