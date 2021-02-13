# Comparisons

## Comparison operators

If `b = 6`, the table below illustrates the comparison operators:

| Operator | Description | Example | Result |
| :--- | :--- | :--- | :--- |
| == | equal to | b == 6 | true |
| != | not equal | b != 6 | false |
| &gt; | greater than  | b &gt; 5 | true |
| &lt; | less than | b &lt; 6 | false |
| &gt;= | greater than or equal to | b &gt;= 6 | true |
| &lt;= | less than or equal to | b &lt;= 6 | true |

### Comparison operator code samples

```javascript
{% assign order_qualifies = false %}

{% if order.cancelled_at == blank and cancel_risk %}
  {% assign order_qualifies = true %}
{% endif %}
```

```javascript
{% if has_product != true %}
  {% log "Order has no products; skipping" %}
  {% assign send_email = false %}
{% endif %}
```

## Contains operator

The `contains` operator is used to check for the existence of a substring in a string or an array of strings.

### Check for the @ sign in a string

```javascript
{% if email_domain contains "@" %}
  {% error "Do not include '@' symbols in email domains. Thanks!" %}
{% endif %}
```

### Check if an array of product\_tags includes a specified tag 

```javascript
{% if product_tags contains options.tag_for_all_other_products %}
  {% assign tag_to_remove = options.tag_for_all_other_products %}
{% endif %}
```

## Logical operators

If `b = 3` and `c = 6`, the table below illustrates the logical operators in Liquid:

| Operator | Example | Result |
| :--- | :--- | :--- |
| and | b &lt; 2 and c &lt; 6 | false |
| or | b &lt; 2 or c &lt; 6 | true |

### Check if both the shipping address and billing are blank

```javascript
{% if order.shipping_address != blank and order.billing_address != blank %}
  {% comment %} do something {% endcomment %}
{% endif %}
```

### Check if tag equals blank or a customer's tags contains a certain tag

```javascript
{% if tag == blank or customer.tags contains tag %}
  {% comment %} do something {% endcomment %}
{% endif %}
```

