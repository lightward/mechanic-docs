# Debouncing events

There are cases when events unavoidably come in more often than is strictly useful. (We've seen this happen when used with [**webhooks**](../platform/webhooks.md), called by a vendor – the vendor may call that webhook far more often than desired.)

To work around this, we can **debounce** events by combining [**event filters**](../platform/events/filters.md) with the [**cache**](../platform/cache/).

{% hint style="info" %}
If you're new to the concept of debouncing: usually encountered in UI implementation, debouncing is the practice of only accepting a single call to a function in a fixed time interval. This is different than throttling, which can develop a backlog by only processing calls at a certain rate – with debouncing, any calls above the rate limit are ignored. Or, in Mechanic's case, they're filtered out.
{% endhint %}

## Configuration

To set up event debouncing, identify the event topic that's receiving excess traffic. In a new task subscribing to that topic (or updating an existing such task), add a Cache action that sets an expiring flag, like this:

{% tabs %}
{% tab title="Task script" %}
```
{% raw %}
{% action "cache" %}
  {
    "setex": {
      "key": "foobar_received",
      "value": true,
      "ttl": 10
    }
  }
{% endaction %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

Choose a cache key and ttl value (in seconds) that make sense for your scenario – the idea is to "remember" that we've received an event of this topic, and to only remember that for a certain amount of time.

Then, in the store's Mechanic settings, add a new event filter, which renders `false` only if the received event has the topic we're interested in, and if that cached value is still in place.

{% tabs %}
{% tab title="Event filter" %}
```
{% raw %}
{% if event.topic == "user/foo/bar" and cache.foobar_received %}
  false
{% else %}
  true
{% endif %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5fc7e48bd580ce55a38b3a41/file-ZywLatxT1R.png)

You're done! Save your settings, and test your work.

## Fingerprinting

The implementation described above identifies and filters events by topic. However, there are many cases where we may want to get even more precise, and identify events to ignore based on data they contain, rather than just by topic. (This can be useful if your events address different resources, like products – you may want to filter out repeated updates to the same product, while allowing updates to previously-unseen products.)

To accomplish this, generate a "fingerprint" of events as you receive them, by assembling the data you're interested in and running it through the sha256 filter, generating a unique string based on the parts of your fingerprint.

{% tabs %}
{% tab title="Task script" %}
```javascript
{% raw %}
{% assign fingerprint_parts = hash %}
{% assign fingerprint_parts["product_id"] = event.data.product_id %}
{% assign fingerprint = fingerprint_parts | json | sha256 %}
{% assign cache_key = "received_" | append: fingerprint %}

{% action "cache" %}
  {
    "setex": {
      "key": {{ cache_key | json }},
      "value": true,
      "ttl": 10
    }
  }
{% endaction %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

Then, bring that logic and resulting cache key over to your event filter.

{% tabs %}
{% tab title="Event filter" %}
```javascript
{% raw %}
{% assign fingerprint_parts = hash %}
{% assign fingerprint_parts["product_id"] = event.data.product_id %}
{% assign fingerprint = fingerprint_parts | json | sha256 %}
{% assign cache_key = "received_" | append: fingerprint %}

{% if event.topic == "user/foo/bar" and cache[cache_key] %}
  false
{% else %}
  true
{% endif %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

Your fingerprint should be composed of data that identifies a resource as perfectly unique. By doing so, you'll be debouncing event resources, instead of an entire event stream.
