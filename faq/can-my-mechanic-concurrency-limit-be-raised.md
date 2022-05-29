# Can my Mechanic concurrency limit be raised?

Perhaps! This question can come up when the Mechanic queue for a particular store becomes backlogged, delaying task activity.

In general, queue slowdowns tend to be related to the Shopify Admin API rate limit, not to the [concurrency limit](../core/runs/concurrency.md). In order to respect the Shopify API rate limit for each store, Mechanic must sometimes artificially slow down its work.

The best place to start is to examine how your tasks interact with the Shopify Admin API, looking for ways to improve efficiency. Often, this looks like migrating things from REST to GraphQL, or looking for places where you're making more requests than necessary.

To learn about maximizing efficiency in this way, see [Shopify / API rate limit](../core/shopify/api-rate-limit.md).

If this direction doesn't prove useful, or if it's not relevant, contact [team@usemechanic.com](mailto:team@usemechanic.com) and we can discuss raising the Mechanic concurrency limit for your store.
