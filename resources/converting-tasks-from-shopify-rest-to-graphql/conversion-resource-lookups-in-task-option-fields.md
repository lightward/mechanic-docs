# Conversion: Resource lookups in task option fields

An oft utilized feature of Mechanic is the ability to add Liquid tags into task options fields, such as a configurable email body. Additionally, these Liquid tags (currently) support inline resource lookups for data not available in the event webhook. However, for products and variants this will no longer work as of the [Feb 1, 2025 REST deprecation date](../../platform/liquid/objects/shopify/).

{% code title="REST - product resource lookup from line item" %}

```liquid
{%- assign qualifying_product = nil -%}

{%- for line_item in order.line_items -%}
  {%- if line_item.product.product_type == "Special" -%}
    {% assign qualifying_product = line_item.product -%}
    {%- break -%}
  {%- endif -%}
{%- endfor -%}

{%- if qualifying_product != blank -%}
  Special product notice for {{ qualifying_product.title }}...
{%- endif -%}
```

{% endcode %}

The code above could be utilized directly in a [multiline task option field](../../core/tasks/options/#flags). and it would output a string of text (e.g. "Special product notice for Widget - Red...") into the assigned option field variable.

One method of conversion for lookup fields is to utilize a GraphQL query _directly in the option field_, which naturally has some caveats.

{% code title="GraphQL - order query with line item products" lineNumbers="true" %}

```liquid
{%- assign order_id = order.admin_graphql_api_id | default: "gid://shopify/Order/12345" -%}

{%- capture query -%}
  query {
    order(id: {{ order_id | json }}) {
      id
      lineItems(first: 250) {
        nodes {
          id
          product {
            title
            productType
          }
        }
      }
    }
  }
{%- endcapture -%}

{%- assign result = query | shopify -%}

{%- assign qualifying_product = nil -%}

{%- for line_item in result.data.order.lineItems.nodes -%}
  {%- if line_item.product.productType == "Special" -%}
    {% assign qualifying_product = line_item.product -%}
    {%- break -%}
  {%- endif -%}
{%- endfor -%}

{%- if qualifying_product != blank -%}
  Special product notice for {{ qualifying_product.title }}...
{%- endif -%}
```

{% endcode %}

{% hint style="warning" %}
Event preview blocks are not evaluated in task option fields. Instead, default values should be assigned to any webhook fields utilized by the query (e.g. _product.admin\_graphql\_api\_id_). This will keep the task parser happy and allow you to save the task. Be careful though to not assign a default value to a webhook field that can have a null or blank string as a valid value.
{% endhint %}

It can be helpful when using a GraphQL query in a task option field to add the code flag to the option field, which will add line numbers and give access to Mechanic code snippets.

```liquid
{% assign email_body = options.email_body__multiline_code_required | strip | newline_to_br %}
```

<div><figure><img src="../../.gitbook/assets/Screenshot 2025-01-13 1.26.00 PM.png" alt=""><figcaption><p>Email body task option</p></figcaption></figure> <figure><img src="../../.gitbook/assets/Screenshot 2025-01-13 1.26.16 PM.png" alt=""><figcaption><p>Email body task option using code flag</p></figcaption></figure></div>

{% hint style="info" %}
The embedded GraphQL query will work without or without using the "code" flag.
{% endhint %}
