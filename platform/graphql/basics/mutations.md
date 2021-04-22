# Mutations

GraphQL mutations create, update, or delete objects in the Shopify GraphQL admin API. A mutation has a name, takes input data, and specifies fields to return in the response.

## Create a product and return the product ID

{% tabs %}
{% tab title="GraphQL mutation" %}
```graphql
mutation {
  productCreate(input: {title: "Red Ball", productType: "Toy", vendor: "Toys"}) {
   product {
    id
   }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```cpp
{
  "data": {
    "productCreate": {
      "product": {
        "id": "gid://shopify/Product/13588"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Update a product's type and return the product ID and new type

{% tabs %}
{% tab title="GraphQL mutation" %}
```graphql
mutation {
  productUpdate(input: {id: "gid://shopify/Product/13588", productType: "Ball"}) {
   product {
    id
    type
   }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```cpp
{
  "data": {
    "productCreate": {
      "product": {
        "id": "gid://shopify/Product/13588"
        "productType" : "Ball"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Great resources for learning GraphQL mutations

{% hint style="success" %}
[https://shopify.dev/concepts/graphql/mutations](https://shopify.dev/concepts/graphql/mutations)
{% endhint %}

{% hint style="success" %}
[https://www.shopify.com/partners/blog/getting-started-with-graphql](https://www.shopify.com/partners/blog/getting-started-with-graphql)
{% endhint %}

{% hint style="success" %}
[https://graphql.org/learn/queries/](https://graphql.org/learn/queries/)
{% endhint %}

