# API rate limit

Mechanic has native awareness of Shopify's **API rate limit**, and will accordingly gate the execution of operations that require access to the Shopify API.

If the rate limit has been reached, any due task runs or [Shopify action](../actions/shopify.md) runs will wait to be enqueued until the rate limit has recovered.

If the rate limit is reached _during_ a run's performance, Mechanic will automatically wait and retry any affected API queries until they succeed, up to a certain number of retries. If the API rate limit does not recover in a reasonable amount of time, Mechanic will raise a permanent error for the run.

## Optimizing API usage

### Query efficiency

When querying for data within a task, use [GraphQL](read/graphql-in-liquid.md) whenever possible, rather than using [Liquid objects](read/liquid-objects.md). GraphQL is much more resource-efficient, and usually results in greater operational throughput.

When working with large volumes of data, use a [bulk operation](read/bulk-operations.md). This way, Shopify bears the burden of collecting all relevant data, without in any way playing against the Shopify API rate limit for Mechanic.

### Task configuration

Keep an eye on tasks that are running at the same time, competing for resources. The Shopify API rate limit is shared across each store's entire Mechanic account, which means that simultaneously-running tasks may be in competition for Shopify API usage. Adjustments to task timing \(possibly using [subscription offsets](../tasks/subscriptions.md#offsets)\) can be useful, when making sure that tasks aren't competing. And, in some cases, it may be useful to decrease the overall [concurrency limit](../runs/concurrency.md) for a Mechanic account, by emailing [team@usemechanic.com](mailto:team@usemechanic.com).

### Shopify Plus

In high-volume scenarios for Shopify Plus accounts, Mechanic's performance can be improved by creating [a private Shopify app](https://help.shopify.com/en/manual/apps/private-apps), having the same permissions that you've granted to Mechanic. Because this private app represents your explicit control and intent, it usually comes with a higher API rate limit. \(And, in some cases, Shopify can grant this private app a _higher_ API usage limit, upon request.\) By providing Mechanic with this private app's API password \(also known as the API secret key, or token\), you can extend this higher limit to Mechanic.

This feature is also useful for accessing Plus-only APIs, which are only available to private Shopify apps. Notably, this includes gift cards \(using [the gift card object](../../platform/liquid/objects/shopify/gift-card.md)\).

This setting can be found in the Mechanic account settings. \(This setting is only available, and is only shown, for Shopify Plus accounts.\) Before adding your API password, you must ensure that the private app has every access scope that Mechanic requires. A list of current required access scopes is provided just below the password field.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e1ae1ef2c7d3a7e9ae61301/5e1ae184f27d5.png)

Once configured, this custom API password will be used for all user-configured Shopify operations, wherever supported. \(It will _not_ be used when querying for publications, since this resource is not accessible to private apps.\)

