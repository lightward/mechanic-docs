# Conversion: Connections from a resource

Until further notice, Shopify will continue to send product webhook data in a REST-like format. Tasks that **only** use the fields available in the webhook (e.g. `product.title`) may not need to be converted by the deprecation notice date. However, if connections to other resources are made from that product (e.g. `product.collections`), then that will require conversion.

This is a simple task to loop through a product's collections, check if the collection contains a certain tag, then log out the collection title.

{% code title="REST - Looping through a product" overflow="wrap" lineNumbers="true" %}

```liquid

{% for collection in product.collections %}
  {% assign collection_tags = collection.tags | split: ", " %}

  {% if collection_tags contains "my-tag" %}
    {% log collection_with_my_tag: collection.title %}
  {% endif %}
{% endfor %}
```

{% endcode %}

***

The GraphQL version of the the task above use a paginated query to get all of the collections a product is a member of. The outer loop upper range (e.g. the **10** in `{% for n in (1..10) %}`) is arbitrary, and you may adjust it to the approximate maximum number of collections any given product might have.

The event preview block in this task sample makes this code appear to be overly verbose, however the [preview block](../../core/tasks/previews/stub-data.md#stubbing-graphql-data) is often an important step to ensure that Mechanic prompts for the correct scopes for reading and writing Shopify API data.

{% code title="GraphQL - Querying a product's collections with pagination" overflow="wrap" lineNumbers="true" %}

```liquid
{% assign cursor = nil %}

{% for n in (1..10) %}
  {% capture query %}
    query {
      product(id: {{ product.admin_graphql_api_id | json }}) {
        collections(
          first: 250
          after: {{ cursor | json }}
        ) {
          pageInfo {
            hasNextPage
            endCursor
          }
          nodes {
            id
            title
            tags
          }
        }
      }
    }
  {% endcapture %}

  {% assign result = query | shopify %}

  {% if event.preview %}
    {% capture result_json %}
      {
        "data": {
          "products": {
            "nodes": [
              {
                "collections": {
                  "nodes": [
                    {
                      "id": "gid://shopify/Collection/1234567890",
                      "title": "Widget collection",
                      "tags": ["my-tag"]
                    }
                  ]
                }
              }
            ]
          }
        }
      }
    {% endcapture %}

    {% assign result = result_json | parse_json %}
  {% endif %}

  {% for collection in result.data.product.collections.nodes %}
    {% if collection.tags contains "my-tag" %}
      {% log collection_with_my_tag: collection.title %}
    {% endif %}
  {% endfor %}

  {% if result.data.products.pageInfo.hasNextPage %}
    {% assign cursor = result.data.products.pageInfo.endCursor %}
  {% else %}
    {% break %}
  {% endif %}
{% endfor %}
```

{% endcode %}

{% hint style="info" %}
To assist with generating a paginated query block, you can use the ["paginated\_query" snippet](../../platform/liquid/mechanic-code-snippets.md#paginated_query) in the Mechanic code editor, and it will prompt you to choose the object type to paginate over (e.g. products).
{% endhint %}
