# How do I add an unsubscribe link to my emails?

Mechanic doesn't permit bulk email messages, in which the same content is sent to many customers at once. As a rule of thumb: if an email requires an unsubscribe link, it shouldn't be sent using Mechanic. \([Learn more about this.](../core/actions/email.md#restrictions)\)

In the rare case that an email _does_ warrant an unsubscribe link, use `{{ customer.unsubscribe_url }}` to add a link, like this:

```text
<a href="{{ customer.unsubscribe_url }}">Unsubscribe</a>
```

This property of [the customer object](../platform/liquid/objects/shopify/customer-object.md) points to a Mechanic-powered URL that...

1. Turns off the "Accepts marketing" property of the related customer, in their Shopify records
2. Gives the customer a link to return them to the Shopify store's online storefront

{% hint style="info" %}
If your task code doesn't have a customer object, you might have to write some different Liquid code to achieve the same ends. The idea is to call the `unsubscribe_url` attribute of the appropriate customer object, regardless of what specific variable holds the customer object.
{% endhint %}

