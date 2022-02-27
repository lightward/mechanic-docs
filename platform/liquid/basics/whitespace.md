# Whitespace

Outside of Liquid statements, all whitespace is preserved. This can result in whitespace that is unwieldy (for example, while formatting GraphQL queries).

To avoid this, use hyphens just inside of Liquid statement openings and closings, as in the following:

* `{{- eats_whitespace_on_the_left }}`
* `{%- assign eats_whitespace_on_the_left = true %}`
* `{{ eats_whitespace_on_the_right -}}`
* `{% assign eats_whitespace_on_the_right = true -%}`
* `{{- eats_whitespace_on_both_sides -}}`
* `{%- assign eats_whitespace_on_both_sides -%}`

{% hint style="info" %}
Learn more from Shopify: see [Whitespace control](https://shopify.dev/docs/themes/liquid/reference/basics/whitespace).
{% endhint %}

## Example

```javascript
{% raw %}
{%- capture message -%}
  This is a message.
{%- endcapture -%}

{% capture message -%}
  This is a message.
{% endcapture -%}
{% endraw %}

{{- customer.name -}}
{{ customer.name -}}
{{- customer.name }}
```
