# I'm getting a "query param length is too long" error when using GraphQL.

This error is about the length of the GraphQL query as a whole (which Shopify limits to 50,000 characters in total), and _not_ about specific strings within the query. (Using an example from the Mechanic task library, [this scenario](https://github.com/lightward/mechanic-tasks/blob/fc08062fb1b861c95f4244c1c12f231646770700/docs/sync-inventory-for-shared-skus/script.liquid#L17-L44) could return this error if the list of list of SKUs is extremely long.)

To work around this error, refactor your query so that any long string values are provided using GraphQL variables, instead of provided inline.

{% hint style="info" %}
Learn about this technique using the [Shopify action](../core/actions/integrations/shopify.md#graphql-with-variables), or with the [shopify Liquid filter](../platform/liquid/filters/#shopify).
{% endhint %}

If your query _isn't_ using any particularly large string values, you may need to split your query into several pieces instead.
