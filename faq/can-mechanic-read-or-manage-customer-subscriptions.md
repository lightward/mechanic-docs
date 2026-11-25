# Can Mechanic read or manage customer subscriptions?

Mechanic tasks cannot use the Shopify Admin API to read or change a subscription contract owned by another app. Shopify scopes contract access to the app that owns it. [Shopify’s subscription contract reference](https://shopify.dev/docs/api/admin-graphql/latest/objects/SubscriptionContract)

Your subscription app may still be able to send events to a task. For example, the [Appstle Subscriptions integration](../platform/integrations/appstle-subscriptions.md) can notify a task when a subscription is created, activated, or cancelled.

{% hint style="info" %}
For community discussion on this subject, see [this Slack thread](https://usemechanic.slack.com/archives/CURE65U2W/p1654980143830719?thread\_ts=1618427015.422500\&cid=CURE65U2W). (If you haven't joined the community Slack workspace yet, join here first: [slack.mechanic.dev](https://slack.mechanic.dev).)
{% endhint %}
