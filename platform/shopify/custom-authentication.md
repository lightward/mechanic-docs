# Custom authentication

In extraordinary cases, Mechanic can be configured to route Shopify API requests through a custom Shopify app, instead of Mechanic's own installation. This may be necessary when a store's Mechanic tasks need to perform work in a way that is only possible via a custom app (e.g. accessing select Shopify APIs, or working with a Shopify API rate limit negotiated for a specific custom app).

{% hint style="info" %}
Most stores never need this. Mechanic's own installation covers the Shopify APIs that tasks use, and options like ["Read all orders"](read-all-orders.md) are available natively. Custom authentication exists for the exceptions.
{% endhint %}

## Configuration

Shopify Plus accounts can configure this in the Permissions section of the store's Mechanic settings, by saving a custom Shopify Admin API access token.

Before saving a new access token, you must ensure that the custom app has every access scope that Mechanic requires.

{% hint style="warning" %}
While a custom access token is saved, Mechanic routes **all** of the store's Shopify API activity through it. If the token becomes invalid (for example, if the custom app is deleted, or its token is rotated), task runs will fail with 401 errors from Shopify — even for work that Mechanic's own authentication could perform.

To return to Mechanic's own authentication, clear the custom access token field and save.
{% endhint %}

## Obtaining an access token

As of January 1, 2026, Shopify no longer supports creating "legacy" custom apps from the Shopify admin ("Apps and sales channels" → "Develop apps"). New custom apps are created in Shopify's [Dev Dashboard](https://shopify.dev/docs/apps/build/authentication-authorization/access-tokens/generate-app-access-tokens-admin) instead.

This matters for Mechanic's custom authentication, because Mechanic stores a single static access token:

* **Existing legacy custom apps** continue to work. If your organization already has one installed on the store, its Admin API access token (beginning with `shpat_`) remains valid, and may be used with Mechanic.
* **Dev Dashboard apps using the client credentials grant** issue tokens that expire every 24 hours. These are **not suitable** for Mechanic, which does not refresh tokens.
* **Dev Dashboard apps installed via OAuth with offline access** yield a long-lived access token that works with Mechanic. This route requires performing Shopify's OAuth authorization code grant flow once, which typically involves a developer.

If you're unsure which route applies to you — or whether you need custom authentication at all — [send us a note](mailto:team@usemechanic.com). :)
