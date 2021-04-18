# Cache endpoints

Cache endpoints are private JSON APIs for accessing data from the Mechanic [**cache**](./). Like [webhooks](../webhooks.md), cache endpoints may be called from a Shopify online storefront \(and from other origins\). This makes for a flexible way to generate, collect, or transform data into something you can pull in to your online customer experiences.

Combining webhooks with cache endpoints creates a closed loop: webhooks receive data for processing, and cache endpoints can return data once processed.

## Configuration

Cache endpoints may be configured in the Mechanic settings:

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e8b9ddc2c7d3a7e9aea78fd/file-ZNE7lcFQXd.png)

Enter the cache key, where the desired data has been stored, and save to receive a unique cache endpoint URL. The URL, when accessed, will return a JSON response containing the value associated with that cache key.

### Alternatives

Not all data can be JSON-encoded. To work around this restriction, use the [base64](../liquid/filters.md#base-64-decode_base64) Liquid filter to cache base64-encoded data.

Values cached in Mechanic have a maximum lifetime of 60 days. If data must be stored long-term, or is quite large, consider using the [FTP](../../core/actions/ftp.md) action in concert with a service like [Couchdrop](https://couchdrop.io/), to send data to a more appropriate storage location.

