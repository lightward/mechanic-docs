# Metafields object

This object allows you to iterate through a resource's metafields, or to access a single metafield directly by performing lookups using a namespace and then a key.

## How to access it

* To retrieve a list of metafields, use `{% for metafield in resource.metafields %}`, for any resource that supports metafields
* To retrieve a list of metafields having a specific namespace, use `{% for metafield in resource.metafields.namespace %}`, for any resource that supports metafields
* To retrieve a specific metafield value, use `{{ resource.metafields.namespace.key }}`, for any resource that supports metafields \(see notes below about values in this usage\)

## What it contains

* Each metafield object contains [every property from the Shopify API](https://help.shopify.com/en/api/reference/metafield)
* Metafield values retrieved via `{{ resource.metafields.namespace.key }}` are automatically parsed and typed according to their `"value_type"`. This is the only time that this happens – in every other usage, metafield values are returned exactly as they are found in the Shopify API.

