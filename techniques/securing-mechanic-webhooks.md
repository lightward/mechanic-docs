# Securing Mechanic webhooks

[Mechanic webhooks](../platform/webhooks.md) are configured properly for CORS, which makes them suitable for submissions from your online store's frontend.

These webhooks do not include any authentication, so you may want to add some verification in your own implementation. One might use something like this, in your store's theme code:

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

With this in place, one would then use something like this, in the corresponding Mechanic task script:

```javascript
{% assign customer_id = event.data.customer_id %}
{% assign customer_id_signature = event.data.customer_id_signature %}

{% assign expected_customer_id_signature = customer_id | hmac_sha256: "some-secret-value" %}

{% if expected_customer_id_signature != customer_id_signature %}
  {% error "Customer ID signature did not match." %}
{% endif %}
```

This way, you can be sure that incoming data was, in fact, prepared by a real Shopify storefront request.

