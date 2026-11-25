# API rate limit

Mechanic has native awareness of Shopify's **Admin API rate limit**, and will accordingly manage the execution of operations that require access to the Shopify API. Mechanic users do not need to manage the API rate limit themselves.

If the rate limit has been reached, any due task runs or [Shopify action](../actions/shopify.md) runs will wait to be enqueued until the rate limit has recovered.

If the rate limit is reached _during_ a run's performance, Mechanic will automatically wait and retry any affected API queries until they succeed, up to a certain number of retries. If the API rate limit does not recover in a reasonable amount of time, Mechanic will raise a permanent error for the run.

{% hint style="info" %}
Learn more about the Admin API rate limit from Shopify, at [https://shopify.dev/api/usage/rate-limits](https://shopify.dev/api/usage/rate-limits).
{% endhint %}

## Optimizing API usage

### Query efficiency

When querying for data within a task, use [GraphQL](read/graphql-in-liquid.md) whenever possible, rather than using [Liquid objects](read/liquid-objects.md). GraphQL is much more resource-efficient, and usually results in greater operational throughput.

When working with large volumes of data, use a [bulk operation](bulk-operations.md). This way, Shopify bears the burden of collecting all relevant data, without in any way playing against the Shopify API rate limit for Mechanic.

### Task configuration

Keep an eye on tasks that are running at the same time, competing for resources. The Shopify API rate limit is shared across each store's entire Mechanic account, which means that simultaneously-running tasks may be in competition for Shopify API usage. Adjustments to task timing (possibly using [subscription offsets](../tasks/subscriptions.md#offsets)) can be useful, when making sure that tasks aren't competing. And, in some cases, it may be useful to decrease the overall [concurrency limit](../runs/concurrency.md) for a Mechanic account, by emailing [team@usemechanic.com](mailto:team@usemechanic.com).

### Shopify Plus

If your Shopify Plus store has a custom app with an API rate limit negotiated with Shopify, you can use that app’s Admin API token for your Mechanic tasks. Check the scopes your tasks require in **Settings → Permissions** before saving the token.

Mechanic uses the custom token for task requests wherever supported. For publication queries that need permissions missing from the custom token, Mechanic can use its own app access. See [Custom authentication](../../platform/shopify/custom-authentication.md) for details.
