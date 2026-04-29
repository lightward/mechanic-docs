# Custom authentication

In extraordinary cases, Mechanic can be configured to route Shopify API requests through a [custom Shopify app](https://help.shopify.com/en/manual/apps/app-types/custom-apps). This may be necessary when a store's Mechanic tasks need to perform work in a way that is only possible via a custom app (e.g. accessing select Shopify APIs, or working with a Shopify API rate limit negotiated for a specific custom app).

## Configuration

Shopify Plus accounts can configure this in the Permissions section of the store's Mechanic settings.

Before saving a new access token, you must ensure that the custom app has every access scope that Mechanic requires.
