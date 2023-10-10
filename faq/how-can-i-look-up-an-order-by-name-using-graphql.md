# How can I look up an order by name using GraphQL?

_A quick clarification, first: the order name is the order number plus any configured prefix/suffix. An order's number might be 123, but its order name might be "#CS123-A"._

Oddly, there isn't a _really good_ way to look up orders by their name. If the name is all you have, you'll have to query for _all_ orders that match the exact order name you have, and then filter for the single order that actually has an exact match for its name. The filter is important: you might get more than one result back, because of other matches in order data.

Use something like this (and see notes below):

```
{% raw %}
{% assign order_name = "#1234" %}
{% assign query = order_name | json %}

{% capture query %}
  query {
    orders(first: 250, query: {{ query | json }}) {
      edges {
        node {
          id
          name
        }
      }
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{% assign order_node = result.data.orders.edges | map: "node" | where: "name", order_name | first %}
{% assign order_id = order_node.id %}
{% endraw %}
```

A couple notes about this:

* The `json` filter is used more than once here, and that's intentional: ultimately it results in something like `query: "\"#1234\""`, which means Shopify will query for `"#1234"`, which means Shopify will search for an _exact_ match of the order name.
* We use `first: 250` here because it'll probably return the order we need. There's a non-zero chance we'd need to look through more than 250 results for a match, but it's (usually) very unlikely.
* The requested cost for this query, as written, is 252 – but, for a single matching order, the actual query cost (i.e. the cost that ultimately counts against your rate limit) is only 3. You could trim the query down to only `first: 5` or something, but it wouldn't make a practical difference in query cost, and it would _increase_ the odds of missing the order you need to find. (For more information on GraphQL query costs, see Shopify's article: [GraphQL Admin API rate limits](https://shopify.dev/concepts/about-apis/rate-limits#graphql-admin-api-rate-limits).)
