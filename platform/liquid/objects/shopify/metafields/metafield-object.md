# Metafield object

{% hint style="info" %}
This page is part of a series: see [Metafields](./) for an overview on how Mechanic's Liquid implementation thinks about metafields.
{% endhint %}

The metafield object reflects the REST Admin API's representation of a metafield.

Mechanic includes support for the metafield object for completeness, but it _is not_ the most useful way to go about using metafield values \(see [Metafield representation object](metafield-representation-object.md)\).

## What it contains

* Every property from [the Metafield resource in the Shopify REST Admin API](https://shopify.dev/api/admin/rest/reference/metafield)

{% hint style="warning" %}
For many metafield types \(e.g. dimension, weight, decimal, etc\), the `value` attribute of the metafield object is a JSON-encoded string. For most scalar types \(e.g. boolean, number\_integer\), the `value` attribute has a matching type.

The simplest way to access a usable version of a metafield value is via the [metafield representation object](metafield-representation-object.md), i.e. `resource.metafields.namespace.key.value`.
{% endhint %}

## How to access it

* For modern metafields, use `resource.metafields.namespace.key.metafield` to retrieve the metafield object itself.
  * Use `resource.metafields.namespace.key` to access the [metafield representation object](metafield-representation-object.md) for that metafield.
* For deprecated metafields, use `resource.metafields.namespace.key` to retrieve the parsed, appropriately-typed metafield value.

