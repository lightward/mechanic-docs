# Iteration

## for tag

Exactly what you'd expect from a for loop in any other programming language.

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

## continue tag

The loop skips its current iteration when it encounters the `continue`tag.

```javascript
{% for fulfillment in order.fulfillments %}
  {% if fulfillment.status == "cancelled" %}
   {% continue %}
  {% endif %}
  {% assign tag_to_add = "not cancelled" %}
{% endfor %}
```

## break tag

The loop stops iterating when it encounters the `break` tag.

```javascript
{% for line_item in order.line_items %}
  {% if line_item.variant.compare_at_price != blank %}
    {% assign purchase_includes_compare_at_pricing = true %}
    {% break %}
  {% endif %}
{% endfor %}
```

## forloop object

The `forloop`object contains information about its parent loop.  This object is handy when working with Mechanic tasks. Learn more [here](https://shopify.dev/docs/themes/liquid/reference/objects/for-loops).

```javascript
{% for lookup in options.product_property_lookups__array_required %}
  {% if forloop.first %}
    {% assign tag = "first" %}
  {% else %}
    {% assign othe_tag = "not first" %}
  {% endif %}
{% endfor %}
```

