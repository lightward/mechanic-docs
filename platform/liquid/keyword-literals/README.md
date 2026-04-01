---
description: >-
  Mechanic's keyword literals for Liquid — array, hash, and newline for working with data structures and formatting.
---

# Keyword literals

Mechanic adds several keyword literals to Liquid — special values that can be used directly in expressions without needing to be assigned or constructed. These make it easier to work with common data structures and formatting.

## Available keywords

* [array](array.md) — an empty array, useful as a starting point for building up a list with `push` or `concat`
* [hash](hash.md) — an empty hash (key-value object), useful for constructing objects property by property
* [newline](newline.md) — a newline character (`\n`), useful with filters like `split` and `join`

## Examples

```liquid
{% comment %} Start with an empty array and add items {% endcomment %}
{% assign tags = array %}
{% assign tags = tags | push: "vip" | push: "wholesale" %}

{% comment %} Build a hash for structured data {% endcomment %}
{% assign customer_data = hash %}
{% assign customer_data["name"] = "Alice" %}
{% assign customer_data["email"] = "alice@example.com" %}

{% comment %} Split a multi-line string into an array of lines {% endcomment %}
{% assign lines = some_text | split: newline %}
```
