# Custom authentication

In high-volume scenarios for Shopify Plus accounts, Mechanic's performance can be improved by creating [a private Shopify app](https://help.shopify.com/en/manual/apps/private-apps), configured for the permissions Mechanic requires. Because this private app represents the store owner's explicit control and intent, Shopify can – at the owner's request – grant it a higher API usage limit. By providing Mechanic with this private app's API password \(also known as the API secret key, or token\), you can extend this higher limit to Mechanic.

This feature is also useful for accessing Plus-only APIs, which are only available to private Shopify apps. Notably, this includes gift cards \(using [the gift card object](../liquid/objects/shopify/gift-card.md)\).

## Configuration

You can find this setting in your Mechanic account manager.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e1ae1ef2c7d3a7e9ae61301/5e1ae184f27d5.png)

{% hint style="info" %}
This setting is only available, and is only shown, for Shopify Plus accounts.
{% endhint %}

Before adding your API password, you must ensure that the private app has every access scope that Mechanic requires. A list of current required access scopes is provided just below the password field.

Once configured, Mechanic will use your custom API password for every [Shopify interaction](../../core/shopify/), except for Admin API calls that require permissions that private apps cannot access \(i.e. calls to Publication resources\).

