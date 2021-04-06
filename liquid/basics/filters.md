# Filters

**Filters** transform a value into something else. Filters are separated by the pipe symbol: `|`. Filters can be chained, performing several transformations in a sequence.

Mechanic inherits [all filters from Liquid itself](https://shopify.github.io/liquid/), and adds several of its own. It does not have all of [Shopify's filters](https://shopify.dev/docs/themes/liquid/reference/filters).

For a complete list of filters supported by Mechanic, see [Filters](../filters.md).

## Example syntax

```javascript
{{ order.billing_address.zip | upcase }}

{% assign full_name = customer.first_name | append: " " | append: customer.last_name %}
```

