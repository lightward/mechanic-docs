# Conversion: Metafield lookups from a resource

For every Shopify resource object that supports metafields, Mechanic has traditionally provided a way to directly access those metafields from the resource using [dot notation](../../../platform/liquid/objects/shopify/metafields/metafield-collection.md). This shortcut will no longer be accessible for product and variant REST resources once they are fully deprecated.

{% code title="REST - product metafield value check" lineNumbers="true" %}
```liquid
{% raw %}
{% assign metafield = product.metafields.custom.my_field %}

{% if metafield.value == "Alpha" %}
  {% log "metafield value matched" %}
{% endif %}
{% endraw %}
```
{% endcode %}

While metafields can be queried directly using their ID, this attribute is not present in the product  webhook data. The standard approach in GraphQL is to query the product resource for the metafield(s) and value(s), passing the `namespace` and `key` as the "key" value, in the same manner as the REST dot notation lookup.

{% code title="GraphQL - product query with metafield and value check" lineNumbers="true" %}
```liquid
{% raw %}
{% capture query %}
  query {
    product(id: {{ product.admin_graphql_api_id | json }}) {
      metafield(key: "custom.my_field") {
        value
      }
    }
  }  
{% endcapture %}

{% assign result = query | shopify %}

{% assign metafield = result.data.product.metafield %}

{% if metafield.value == "Alpha" %}
  {% log "metafield value matched" %}
{% endif %}
{% endraw %}
```
{% endcode %}
