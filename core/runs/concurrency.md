# Concurrency

In general, Mechanic will process as many [**runs**](./) simultaneously as possible. This means that multiple tasks subscribing to the same event topic are very likely to execute simultaneously, when such an event occurs.

To protect the health of the system and to ensure performance for every store on the platform, Mechanic have several **concurrency** limits, defining the conditions in which Mechanic will perform runs simultaneously.

## Limits

* Each store's Mechanic account has a fixed run queue size, currently defaulting 5. \(This default is subject to change. Request a different limit by emailing team@usemechanic.com.\) This limit controls how many runs Mechanic will perform simultaneously for your store. With a limit of 5, this could mean 5 events, or 5 tasks, or 1 event and 2 tasks and 2 actions, or any other combination of runs. Additional runs will be performed as the preceding runs complete.
* Mechanic will wait to perform a [Shopify action](../actions/shopify.md) until it is allowed to do so by the Shopify API rate limit for your store. [Read more about this, from Shopify.](https://help.shopify.com/en/api/getting-started/understanding-api-rate-limits)

## Tips

* To ensure that your tasks are working with up-to-date information, consider reloading your resources at the beginning of the task script. For example, a task responding to a customer event might include `{% assign customer = customer.reload %}` at the very beginning, to ensure that any changes to the customer \(since the original event\) are reflected in the `customer` variable.
* Use GraphQL to query Shopify, to keep your data usage efficient. \(To learn more, see [Interacting with Shopify](../interacting-with-shopify/).\)
* For options for ordering execution of runs, see [How can I control run order?](https://docs.usemechanic.com/article/444-how-can-i-control-run-order).

