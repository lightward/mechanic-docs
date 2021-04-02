# Location object

## How to access it

* Use `{% for location in shop.locations %}` in any task
* Use `{{ order.location.name }}` in tasks responding to shopify/orders events
* Use `{{ fulfillment.location.name }}` in tasks responding to shopify/fulfillments events
* Use `{{ inventory_level.location.name }}`  in tasks responding to shopify/inventory\_levels events

## What it contains

* [Every property from the Shopify API](https://help.shopify.com/en/api/reference/inventory/location#properties)

