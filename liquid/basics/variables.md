# Variables

## Assign tag

The assign tag creates a new variable. 

```javascript
{% assign my_name = "Matt" %}

{% assign is_tired = false %}

{% assign favorite_number = 8 %}
```

## Capture tag

The capture tag is a useful tool for creating strings of multiple variables, which is very useful in Mechanic. We use this tag extensively in our task code for capturing GraphQL queries, email bodies, and more.

{% hint style="info" %}
In this example, the string is captured into a variable named query.
{% endhint %}

```javascript
{% capture query %}
  query {
    inventoryLevel(id: {{ inventory_level.admin_graphql_api_id | json }}) {
      item {
        variant {
          sku
        }
      }
    }
  }
{% endcapture %}
```

