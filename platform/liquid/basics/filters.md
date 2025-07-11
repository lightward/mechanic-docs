# Filters

**Filters** transform a value into something else. Filters are separated by the pipe symbol: `|`. Filters can be chained, performing several transformations in a sequence.

{% hint style="info" %}
See [Mechanic filters](../filters/) for a complete list of filters supported by Mechanic Liquid.
{% endhint %}

{% hint style="warning" %}
Liquid filters should not be confused with [event filters](../../events/filters.md), which are used to conditionally ignore incoming events.
{% endhint %}

## Example syntax

```javascript
{{ order.billing_address.zip | upcase }}

{% assign full_name = customer.first_name | append: " " | append: customer.last_name %}
```
