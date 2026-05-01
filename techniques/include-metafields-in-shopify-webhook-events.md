---
description: Include Shopify metafields in webhook event payloads, and optionally filter by metafield values, using Mechanic custom Shopify webhooks.
---

# Include metafields in Shopify webhook events

Most tasks should start with regular `shopify/...` subscriptions. If a task needs a known metafield as soon as a Shopify webhook arrives, a [custom Shopify webhook](../platform/shopify/custom-webhooks.md) can ask Shopify to include that metafield in the webhook payload before Mechanic receives it. Custom Shopify webhooks can also ask Shopify to filter deliveries by metafield data.

Use this when you know which metafield namespace and key you need, and you want to avoid a follow-up GraphQL lookup during the task run. If the task needs flexible metafield queries, related resources, or the freshest current state, use the [Shopify action](../core/actions/shopify.md) or [GraphQL in Liquid](../core/shopify/read/graphql-in-liquid.md) instead.

{% stepper %}
{% step %}

## Create the custom Shopify webhook

Open **Settings**, choose **Custom Shopify webhooks**, and create a webhook for the Shopify topic you need.

```text
Name:           Products tracked by Mechanic
Shopify topic:  shopify/products/update
Mechanic topic: user/products/tracked_update
Filter:         id:*
```

`id:*` keeps every delivery while giving Shopify a non-empty filter. Use a narrower filter when you only want matching resources.

{% endstep %}
{% step %}

## Choose the metafields Shopify should send

Use **Metafields** for exact metafields:

```text
custom.track_with_mechanic
```

Use **Metafield namespaces** when you want every metafield in a namespace:

```text
custom
```

{% hint style="warning" %}
These settings select the metafields Shopify should attach to the webhook subscription. You can also filter on selected metafields with fields like `metafields.namespace`, `metafields.key`, and `metafields.value`.
{% endhint %}

{% hint style="warning" %}
`include_fields` is not a metafield selector. If you use **Include fields** and also want metafields, include `metafields` as one of the fields, then choose the namespace or exact metafields below.
{% endhint %}

Example:

```text
Include fields:
id
title
metafields

Metafields:
custom.track_with_mechanic
```

{% endstep %}
{% step %}

## Optional: filter by the metafield

Use `metafields.namespace`, `metafields.key`, and `metafields.value` when Shopify should only send resources with a matching metafield.

```text
Filter:
metafields.namespace:custom AND metafields.key:track_with_mechanic AND metafields.value:true

Include fields:
id
title
updated_at
metafields

Metafields:
custom.track_with_mechanic
```

For a filter that only checks whether the metafield exists, keep the namespace and key terms and use `metafields.value:*`.

{% hint style="info" %}
Metafield filters are state filters. A `products/update` webhook with `metafields.value:true` means the product currently has that metafield value. It does not mean the metafield changed in this update.
{% endhint %}

{% endstep %}
{% step %}

## Subscribe a task to the Mechanic topic

Tasks subscribe to the custom `user/...` topic, not to the original Shopify topic.

```text
user/products/tracked_update
```

In task code, Mechanic still builds the usual Shopify subject variable from the source topic. For `shopify/products/update`, use `product`.

```liquid
{% assign tracked_by_mechanic = false %}

{% for metafield in product.metafields %}
  {% if metafield.namespace == "custom" and metafield.key == "track_with_mechanic" %}
    {% assign tracked_by_mechanic = metafield.value %}
  {% endif %}
{% endfor %}

{% log
  topic: event.topic,
  shopify_topic: event.shopify_topic,
  product_id: product.id,
  tracked_by_mechanic: tracked_by_mechanic
%}
```

{% endstep %}
{% step %}

## Test with a real Shopify update

Preview mode is useful for task code, but it does not prove Shopify's webhook payload shape. Save the custom Shopify webhook, trigger a matching Shopify update, then inspect the resulting event in Mechanic.

Check:

* `event.topic` is your `user/...` topic
* `event.shopify_topic` is the original Shopify topic
* `event.data.metafields` contains the metafield Shopify delivered

If your webhook uses **Include fields** and you want every matching update to arrive, include `updated_at`. If you leave `updated_at` out, Shopify may skip a recent repeat delivery when the trimmed payload would be identical.

{% endstep %}
{% endstepper %}

## Related

* [Custom Shopify webhooks](../platform/shopify/custom-webhooks.md)
* [Subscriptions](../core/tasks/subscriptions.md)
* [Event object](../platform/liquid/objects/event.md)
* [Reading Shopify data with GraphQL](../core/shopify/read/graphql-in-liquid.md)
