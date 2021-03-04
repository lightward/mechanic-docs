# Pagination

Shopify uses cursor-based pagination and this is a concept you will likely use frequently in your Mechanic tasks. Shopify has excellent coverage on the topics and I highly recommend reviewing their [documentation](https://shopify.dev/concepts/graphql/pagination).

In the query below you'll notice`pageInfo` and `cursor`. These are important concepts for pagination, make sure to review them in the resources listed below. Page info tells us if there is another page of results before or after the current page.  The `cursor` field provides a reference to an edge's position, which is then used as an anchor to retrieves nodes before or after it in the connection. 

```graphql
query {
  orders(first:10) {
    pageInfo { # Returns details about the current page of results
      hasNextPage # Whether there are more results after this page
      hasPreviousPage # Whether there are more results before this page
    }
    edges {
      cursor # A marker for an edge's position in the connection
      node {
        name # The fields to be returned for each node
      }
    }
  }
}
```

### Great resources for learning GraphQL pagination

#### Key concepts to read up on

* [ ] Connections
* [ ] Edges
* [ ] Nodes
* [ ] pageInfo
* [ ] hastNextPage
* [ ] hasPreviousPage
* [ ] cursor
* [ ] Cursor-based pagination

{% hint style="success" %}
[https://shopify.dev/concepts/graphql/pagination](https://shopify.dev/concepts/graphql/pagination)
{% endhint %}

{% hint style="success" %}
[https://graphql.org/learn/pagination/](https://graphql.org/learn/pagination/)
{% endhint %}

