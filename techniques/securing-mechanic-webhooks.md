# Securing Mechanic webhooks

[Mechanic webhooks](../platform/webhooks.md) are configured properly for CORS, which makes them suitable for submissions from your online store's frontend.

These webhooks do not include any authentication, so you may want to add some verification in your own implementation.

### Generating signatures

By establishing a shared secret value, stored in server-side Shopify theme code and in server-side Mechanic task code, the webhook request data can be "signed" with a signature value, uniquely determined by the shared secret combined with the request data. This prevents would-be attackers from modifying the webhook data and re-using the same signature, because any modification to the webhook data would invalidate the signature.

One might use something like this in the Shopify theme code:

```markup
<script>
  $.post(
    'https://usemechanic.com/webhook/000000000',
    {
      customer_id: {{ customer.id | json }},
      customer_id_signature: {{ customer.id | hmac_sha256: "some-secret-value" | json }}
    }
  );
</script>
```

... and then something like this, in the corresponding Mechanic task code:

```javascript
{% assign customer_id = event.data.customer_id %}
{% assign customer_id_signature = event.data.customer_id_signature %}

{% assign expected_customer_id_signature = customer_id | hmac_sha256: "some-secret-value" %}

{% if expected_customer_id_signature != customer_id_signature %}
  {% error "Customer ID signature did not match." %}
{% endif %}
```

### Preventing replay attacks

The approach above prevents would-be attackers from passing off altered data as legitimate, but it does not prevent manually repeated submissions of the _same_ data.

The best approach for preventing this is to ensure that the Mechanic task code is fully idempotent, such that it only performs the desired work _once_, no matter how many times it is invoked.

One way to solve this is to leverage [the Mechanic cache](../platform/cache/) to remember that a given request has already been processed. Here's an example:

```javascript
{% assign cache_key = event.data | json | sha256 %}

{% if cache[cache_key] %}
  {% log "we've already seen this request; skipping it" %}
  {% break %}
{% endif %}

{% action "cache", "set", cache_key, true %}

[... proceed with processing the event]
```

In this way, later submissions of the same webhook data will be ignored.

