# Conversion: Single resource lookups

At its core, accessing a single resource via either API is effectively the same. Typically this involves passing the ID of the resource to the API and getting back the data for that resource.

{% code title="REST - simple product lookup" %}
```
```
{% endcode %}

{% code title="GraphQL - simple product query" overflow="wrap" lineNumbers="true" %}
````

</div>

***

In a REST call, every field of that resource will be returned, allowing the usage of simple dot notation to utilize whichever fields are desired without first requesting them.

<div data-gb-custom-block data-tag="code" data-title='REST - example product logging' data-lineNumbers='true'>
```liquid
{% assign product = shop.products[product_id] %}

{% log
  title: product.title,
  status: product.status,
  type: product.product_type,
  description: product.body_html,
  tags: product.tags,
  image: product.image.src
%}
````
{% endcode %}

The equivalent query in GraphQL would need to be augmented to include the desired fields.

{% hint style="warning" %}
Occasionally, the REST and GraphQL APIs do not use the same field names. And in some cases, there are some fields with no counterpart between the APIs. Review the API docs in detail for the resource being queried to make sure the task code is using the correct field names.
{% endhint %}

{% code title="GraphQL - example product logging" lineNumbers="true" %}
````

</div>

***

This is a basic task to check a product's status, type, and tags, and then output a log entry if that product qualifies.

The preview block is only showing the fields from the REST product webhook that will be used in the task. In reality, there are about 150+ lines of detail from a product webhook which has only a single variant and image. This grows much larger as variants and images are added to the product.

<div data-gb-custom-block data-tag="code" data-title='REST - Basic product tagging task' data-overflow='wrap' data-lineNumbers='true'>
```liquid
{% if event.preview %}
  {% capture product_json %}
    {
      "admin_graphql_api_id": "gid://shopify/Product/1234567890",
      "product_type": "Widget",
      "status": "active",
      "tags": "my-tag, some-other-tag"
    }
  {% endcapture %}

  {% assign product = product_json | parse_json %}
{% endif %}

{% assign product_tags = product.tags | split: ", " %}

{% if product.status == "active" and product.product_type == "Widget" %}
  {% if product.tags contains "my-tag" %}
    {% log
      message: "This product qualifies",
      product: product
    %}
  {% endif %}
{% endif %}
````
{% endcode %}

***

The product id used in the GraphQL query below comes from the REST-like product webhook, which will still exist after the REST product endpoint deprecation.

The preview block simulates the relevant shape of the returned data, which typically matches exactly what was requested in the query. This could vary though based on the task logic following the preview block.

{% code title="GraphQL - Basic product tagging task" overflow="wrap" lineNumbers="true" %}
```

</div>

<div data-gb-custom-block data-tag="hint" data-style='info'>

To assist with generating an object query block, you can use the ["object\_query" snippet](../../platform/liquid/mechanic-code-snippets.md#object_query) in the Mechanic code editor, and it will prompt you to choose the object type to generate a query and preview block for (e.g. product).

</div>

To see a code diff from a Mechanic library task that was recently converted in this manner, click [here](https://github.com/lightward/mechanic-tasks/pull/393/files#diff-e02b657fe67dbee68d890ad84b721837f25f6ab8c99d78ac39f28ef179478228), and review the code variations between the `{% if event.topic == "shopify/orders/create" %}` blocks.
```
{% endcode %}
