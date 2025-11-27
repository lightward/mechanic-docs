# Conversion: Resource loops to paginated queries

A typical REST products loop in Mechanic will have the structure below. While this is a concise format to get all products in shop, its main drawback is the inability to limit or filter the number of records and fields returned. This generates a significant amount of extra data for the task to manage in memory during a task run, especially if connected resources are looped as well (e.g. variants).

{% code title="REST - shop.products resource loop" overflow="wrap" lineNumbers="true" fullWidth="false" %}
````

</div>

***

GraphQL paginated queries work by using the same (potentially filtered) query repeatedly to retrieve resources until the end of the list is reached or the querying is terminated by code logic. In Mechanic, paginated queries are typically implemented by using an outer "for loop", with an arbitrary number of maximum loops (e.g. the **100** in `{% for n in (1..100) %}`).\
\
Within the query itself, the `first` filter limits the number of records returned in this batch, and the `after` filter will instruct which "cursor" the query should start at. This cursor will initially be set to  `nil`, which indicates starting at the beginning, and it will be updated by the looping logic before the next query is run, using `{% assign cursor ... %}`

.&#x20;

<div data-gb-custom-block data-tag="hint" data-style='info'>

Shopify limits most GraphQL resources to 250 records per query, so this will be the most frequent value for the `first` filter seen in tasks using paginated queries.

</div>

Finally, the `query` filter of a resources query gives the ability to drastically reduce the number of records returned, allowing for very targeted inclusion and exclusion rules (e.g. products having a certain tag). Each resource has its own list of query filters, which can be reviewed in the [GraphQL Admin API](https://shopify.dev/docs/api/admin-graphql/) docs

<div data-gb-custom-block data-tag="hint" data-style='warning'>

If a query has the potential to return a very large number of resources (including connected resources) in a shop, then a [bulk operation](../../core/shopify/read/bulk-operations.md) query may be better suited than using paginated GraphQL queries.

</div>

### GraphQL Paginated Query

<div data-gb-custom-block data-tag="code" data-title='GraphQL - paginated products query' data-overflow='wrap' data-lineNumbers='true' data-fullWidth='false'>
```liquid
{% assign cursor = nil %}
{% assign search_query = nil %}

{% for n in (1..100) %}
  {% capture query %}
    query {
      products(
        first: 250
        after: {{ cursor | json }}
        query: {{ search_query | json }}
      ) {
        pageInfo {
          hasNextPage
          endCursor
        }
        nodes {
          id
          # relevant product fields
          variants(first: 100) {
            nodes {
              id
              # relevant variant fields
            }
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
                "id": "gid://shopify/Product/1234567890",
                "variants": {
                  "nodes": [
                    {
                      "id": "gid://shopify/ProductVariant/1234567890"
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

  {% for product in result.data.products.nodes %}
    {% comment %}
      -- product processing here, using GraphQL fields from the query
    {% endcomment %}

    {% for variant in product.variants.nodes %}
      {% comment %}
        -- variant processing here, using GraphQL fields from the query
      {% endcomment %}      
    {% endfor %}
  {% endfor %}
  
  {% comment %}
    -- if there is another page of data, then update the cursor for the next loop
  {% endcomment %}

  {% if result.data.products.pageInfo.hasNextPage %}
    {% assign cursor = result.data.products.pageInfo.endCursor %}
  {% else %}
    {% break %}
  {% endif %}
{% endfor %}
````
{% endcode %}

{% hint style="info" %}
To assist with generating a paginated query block, you can use the ["paginated\_query" snippet](../../platform/liquid/mechanic-code-snippets.md#paginated_query) in the Mechanic code editor, and it will prompt you to choose the object type to paginate over (e.g. products).
{% endhint %}

To see a code diff from a Mechanic library task that was recently converted in this manner, click [here](https://github.com/lightward/mechanic-tasks/pull/393/files#diff-2efeafa8d41fb00ed8ffcd8481f358850d69d8b7537364d17cc744ec9f357681).
