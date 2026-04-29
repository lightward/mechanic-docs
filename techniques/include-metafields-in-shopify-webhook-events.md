---
description: Include Shopify metafields in webhook event payloads using Mechanic custom Shopify webhooks.
---

# Include metafields in Shopify webhook events

Most tasks should start with regular `shopify/...` subscriptions. If a task needs a known metafield as soon as a Shopify webhook arrives, a [custom Shopify webhook](../platform/shopify/custom-webhooks.md) can ask Shopify to include that metafield in the webhook payload before Mechanic receives it.

Use this when you know which metafield namespace and key you need, and you want to avoid a follow-up GraphQL lookup during the task run. If the task needs flexible metafield queries, related resources, or the freshest current state, use the [Shopify action](../core/actions/shopify.md) or [GraphQL in Liquid](../core/shopify/read/graphql-in-liquid.md) instead.

{% stepper %}
{% step %}

## Create the custom Shopify webhook

Open **Settings**, choose **Custom Shopify webhooks**, and create a webhook for the Shopify topic you need.

```text
Name:           Product pack size updates
Shopify topic:  shopify/products/update
Mechanic topic: user/products/pack_size_update
Filter:         id:*
```

`id:*` keeps every delivery while giving Shopify a non-empty filter. Use a narrower filter when you only want matching resources.

{% endstep %}
{% step %}

## Choose the metafields Shopify should send

Use **Metafields** for exact metafields:

```text
custom.pack_size
```

Use **Metafield namespaces** when you want every metafield in a namespace:

```text
custom
```

{% hint style="warning" %}
These settings add metafields to the delivered webhook payload. Shopify does not currently treat metafields as webhook filter fields, so filter on ordinary webhook payload fields first, then inspect metafields in Liquid.
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
custom.pack_size
```

{% endstep %}
{% step %}

## Subscribe a task to the Mechanic topic

Tasks subscribe to the custom `user/...` topic, not to the original Shopify topic.

```text
user/products/pack_size_update
```

In task code, Mechanic still builds the usual Shopify subject variable from the source topic. For `shopify/products/update`, use `product`.

```liquid
{% assign pack_size = nil %}

{% for metafield in product.metafields %}
  {% if metafield.namespace == "custom" and metafield.key == "pack_size" %}
    {% assign pack_size = metafield.value %}
  {% endif %}
{% endfor %}

{% log
  topic: event.topic,
  shopify_topic: event.shopify_topic,
  product_id: product.id,
  pack_size: pack_size
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

{% endstep %}
{% endstepper %}

## Related

* [Custom Shopify webhooks](../platform/shopify/custom-webhooks.md)
* [Subscriptions](../core/tasks/subscriptions.md)
* [Event object](../platform/liquid/objects/event.md)
* [Reading Shopify data with GraphQL](../core/shopify/read/graphql-in-liquid.md)
