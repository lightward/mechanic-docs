---
description: Subscribe Mechanic tasks to Shopify metaobject create, update, and delete events.
---

# Subscribe to Shopify metaobject events

Mechanic's regular `shopify/...` task subscriptions do not include Shopify metaobject topics. To run a task when a metaobject is created, updated, or deleted, create a [custom Shopify webhook](../platform/shopify/custom-webhooks.md) and route the Shopify delivery onto a `user/...` topic.

{% stepper %}
{% step %}

## Create a custom Shopify webhook

Open **Settings**, choose **Custom Shopify webhooks**, and choose the metaobject Shopify topic you need:

```text
shopify/metaobjects/create
shopify/metaobjects/update
shopify/metaobjects/delete
```

Give the webhook a Mechanic topic for tasks to subscribe to:

```text
user/metaobjects/product_review_update
```

{% endstep %}
{% step %}

## Add the required type filter

Shopify requires metaobject webhooks to name the metaobject type. Use a concrete `type:` filter:

```text
type:product_review
```

To listen for more than one type with the same webhook:

```text
type:product_review OR type:faq_entry
```

Shopify does not support `type:*` for metaobject webhooks.

{% endstep %}
{% step %}

## Subscribe the task to the user topic

Tasks subscribe to the Mechanic topic, not the Shopify metaobject topic.

```text
user/metaobjects/product_review_update
```

Mechanic keeps the source Shopify topic in `event.shopify_topic`, and builds a `metaobject` variable from the payload Shopify delivered.

```liquid
{% log
  topic: event.topic,
  shopify_topic: event.shopify_topic,
  metaobject_id: metaobject.id,
  metaobject_handle: metaobject.handle,
  metaobject_type: metaobject.type,
  fields: metaobject.fields
%}
```

{% endstep %}
{% step %}

## Update access if prompted, then test a real change

If the webhook shows **Needs permissions**, Mechanic should prompt you to update Shopify access when it detects missing scopes. The Permissions screen shows granted and required scopes, but it does not grant scopes directly. If a needed scope is not being detected from the task or webhook, declare it in the subscribed task with the [`permissions` tag](../platform/liquid/tags/permissions.md).

Then create, update, or delete a matching metaobject in Shopify. Open the resulting Mechanic event and confirm:

* `event.topic` is your `user/...` topic
* `event.shopify_topic` is the Shopify metaobject topic
* `metaobject` and `event.data` contain the payload Shopify sent

{% endstep %}
{% endstepper %}

## Common gotchas

**Subscribe to the `user/...` topic.** A task subscribed to `shopify/metaobjects/update` will not receive this custom Shopify webhook. Use the Mechanic topic you configured.

**Use a real type filter.** Metaobject webhooks need a concrete `type:` filter. Use separate webhooks or an `OR` filter when different types should route differently.

**Preview is not an end-to-end webhook test.** Preview mode helps verify Liquid behavior, but Shopify filters and Shopify payload customization are only proven by a real matching Shopify event.

## Related

* [Custom Shopify webhooks](../platform/shopify/custom-webhooks.md)
* [Event topics](../platform/events/topics.md)
* [Subscriptions](../core/tasks/subscriptions.md)
* [Permissions](../core/tasks/permissions.md)
