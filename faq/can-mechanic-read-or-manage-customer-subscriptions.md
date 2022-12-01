# Can Mechanic read or manage customer subscriptions?

No. Shopify _does_ have well-developed [APIs for customer subscription contracts](https://shopify.dev/api/admin-graphql/current/objects/SubscriptionContract), but they don't currently allow one app to read or manage subscriptions created by another app. (Shopify _does_ have a `read_all_subscription_contracts` scope, but they haven't elected to approve this scope for any third-party apps just yet.)

If this affects your business, please let Shopify know! We'd love to have Mechanic be useful in this area too. :)

{% hint style="info" %}
For community discussion on this subject, see [this Slack thread](https://usemechanic.slack.com/archives/CURE65U2W/p1654980143830719?thread\_ts=1618427015.422500\&cid=CURE65U2W). (If you haven't joined the community Slack workspace yet, join here first: [slack.mechanic.dev](https://slack.mechanic.dev).)
{% endhint %}
