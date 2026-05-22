---
description: Import files for library tasks that use custom Shopify webhooks
---

# Custom webhook imports

### [Monitor for variants with zero price](https://tasks.mechanic.dev/monitor-for-variants-with-zero-price)

This task requires importing this custom Shopify webhook or creating it manually.

{% code title="variants-with-zero-price-custom-webhook.json" %}
```json
{
  "name": "Variants with zero price",
  "shopify_topic": "shopify/products/update",
  "event_topic": "user/product_update/variants_zero_price",
  "filter": "variants.price:\"0.00\" AND status:active AND published_at:*",
  "include_fields": [
    "id",
    "updated_at",
    "title",
    "status",
    "published_at",
    "variants.id",
    "variants.updated_at",
    "variants.title",
    "variants.sku",
    "variants.price"
  ],
  "metafield_namespaces": [],
  "metafields": []
}
```
{% endcode %}

