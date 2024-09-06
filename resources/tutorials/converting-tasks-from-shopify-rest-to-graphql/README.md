---
icon: triangle-exclamation
---

# Converting tasks from Shopify REST to GraphQL

{% hint style="danger" %}
**Shopify is deprecating some of the Shopify Admin REST API. The first round of deprecations involve the product and variant endpoints. Read the deprecation notice** [**here**](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing)**.**

Our recommendation is to use [GraphQL](../../../core/actions/shopify.md#graphql) going forward. The [product](../../../platform/liquid/objects/shopify/product.md) and [variant](../../../platform/liquid/objects/shopify/variant.md) objects will cease to work on on Feb 1, 2025 due to the changes being made by Shopify. It also appears that Shopify will gradually phase out the REST API.

Over time, all of our [library tasks](https://tasks.mechanic.dev/) will be ported to use GraphQL only, which in conjunction with these tutorials, will provide a model for how you can update your custom tasks.&#x20;
{% endhint %}

These conversion tutorials will be be based on products, variants, and associated resources, but the methodologies are applicable to other type of REST resources as well.

#### At a high-level, converting Mechanic tasks from Shopify REST lookups to GraphQL queries involves:

1. **Understanding the Shopify GraphQL schema**\
   Familiarize yourself with the [Shopify GraphQL Admin API](https://shopify.dev/docs/api/admin-graphql) objects, queries, and mutations.
2. **Review how to use GraphQL in Mechanic**\
   Start [here](https://learn.mechanic.dev/core/shopify/read/graphql-in-liquid) and peruse the [task library](../../task-library/) to see examples of GraphQL usage in tasks.
3. **Identify REST usage within a task**\
   Broadly, any usage where **one Liquid REST object** is used to reference another Liquid REST object with dot notation. This does not include fields on the original REST-like webhook resource (e.g. `product.title`).\
   \
   For the product and variant resource deprecations specifically, this includes:
   * `shop.products`
   * `shop.variants`
   * `collection.products`
   * `inventory_item.variant`
   * `inventory_level.variant`
   * `line_item.product`
   * `line_item.variant`
   * `product.collections`
   * `product.images` :asterisk\_symbol:
   * `product.metafields`
   * `product.variants` :asterisk\_symbol:
   * `variant.inventory_item`
   * `variant.inventory_levels`
   * `variant.metafields`
   * `variant.product`
4. **Field mapping**: Identify the objects, fields, and nested structures needed in GraphQL based on the existing REST usage within a task. Build and validate queries using [Shopify's GraphiQL Explorer](../../../platform/graphql/basics/shopify-admin-api-graphiql-explorer.md).
5. **Update Mechanic task code** : Replace the relevant REST calls with Mechanic-flavored Liquid GraphQL query and result objects (see the tutorials following this page for examples).
6. **Testing**: Trigger the updated task to make sure it returns the expected results and/or takes the expected actions.

{% hint style="warning" %}
The product webhook does include an array of images and variants in the product JSON which will still be accessible via dot notation. Note that these are not the same as the previously available Mechanic REST lookups for those resources.

The images and variants data arrays should be used with caution once Shopify releases [support for 2 thousand variants per product](https://shopify.dev/docs/apps/build/graphql/migrate/new-product-model#whats-changing), in conjunction with the product and variant REST endpoint deprecations. The product webhook will only include full detail for the first 100 variants. It is not yet clear what Shopify will do with images in the product webhook.
{% endhint %}
