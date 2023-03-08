# "Read all orders"

When order access is required, Mechanic defaults to requesting access to the last 60 days of a store's order history. Users can choose to give Mechanic access to all orders, across the store's entire history, by enabling the "Read all orders" option.

{% hint style="info" %}
#### Why?

Mechanic has a platform policy of requiring as few permissions as necessary. As such, it uses Shopify's `read_orders` OAuth scope, which covers the last 60 days of order history. Enabling Mechanic's "Read all orders" setting results in Mechanic requesting the `read_all_orders` scope instead.
{% endhint %}

## Configuration

Find the "Read all orders" option by opening Mechanic's settings, via the "Settings" link in the main app navigation. Or, navigate directly to [https://admin.shopify.com/apps/mechanic/settings](https://admin.shopify.com/apps/mechanic/settings).

<figure><img src="../../.gitbook/assets/Screen Shot 2022-09-25 at 9.43.15 AM.png" alt=""><figcaption></figcaption></figure>
