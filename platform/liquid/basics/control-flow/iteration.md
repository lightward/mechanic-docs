# Iteration

**Iteration** tags control the flow of code, running that code multiple times against successive values in an array.

## for

The `for` tag runs its code against every value found in an array.

```javascript
{% for product in products %}
  {% assign product_tags = product.tags | split: ", " %}
{% endfor %}
```

```javascript
{% assign seconds_per_day = 86400 %}

{% for i in (1..days_ahead) %}
  {% assign time_ahead = 0 | plus: seconds_per_day | times: i %}
{% endfor %}
```

### The forloop object

The `forloop`object contains information about the loop currently being processed. [Learn about its contents from Shopify](https://shopify.dev/docs/themes/liquid/reference/objects/for-loops).

```javascript
{% for tag in tags %}
  {% if forloop.first %}
    {% comment %}
      This is the first tag in the set!
    {% endif %}
  {% endif %}
{% endfor %}
```

## continue

Used within a `for`/`endfor` block, the `continue` tag instructs Liquid to skip the rest of the code in the current pass, and begin again with the next value in the array (if any).

```javascript
{% for fulfillment in order.fulfillments %}
  {% if fulfillment.status == "cancelled" %}
    {% comment %}
      Skip this fulfillment, and process the next one (if any).
    {% endcomment %}
    {% continue %}
  {% endif %}

  {% assign tag_to_add = "not cancelled" %}
{% endfor %}
```

## break

Used within a `for`/`endfor` block, the `break` tag instructs Liquid to skip the rest of the code in the current pass, and to stop processing the array completely.

```javascript
{% assign order_includes_compare_at_pricing = false %}

{% for line_item in order.line_items %}
  {% if line_item.variant.compare_at_price != blank %}
    {% assign order_includes_compare_at_pricing = true %}
    {% comment %}
      Only need to detect this once, so we can now break out of the loop.
    {% endcomment %}
    {% break %}
  {% endif %}
{% endfor %}
```
