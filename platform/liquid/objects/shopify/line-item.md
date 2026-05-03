# 🚫 Line item object

{% hint style="warning" %}
These Shopify REST-backed Liquid objects are legacy. The product and variant Liquid objects no longer work, and Shopify is phasing out the REST Admin API that these objects depend on. Use [GraphQL](../../../../core/actions/shopify.md#graphql) for new work; see the [Shopify REST Admin API notice](README.md) and [migration guides](../../../../resources/converting-tasks-from-shopify-rest-to-graphql/).
{% endhint %}

## How to access it

* Access an array of line items using `{{ order.line_items }}` whenever [an order object](order.md) is available

## What it contains

* Every property from Orders.line\_items in the Shopify REST Admin API
* The related [product object](product.md): `{{ line_item.product }}`&#x20;
* The related [variant object](variant.md): `{{ line_item.variant }}`&#x20;
* An array of properties, that also supports lookups by attribute name: `{% for prop in line_item.properties %}{% if prop.name == "Delivery window" %}{{ prop.value }}{% endif %}{% endfor %}`, or `{{ line_item.properties["Delivery window"] }}`&#x20;
