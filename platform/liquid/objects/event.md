# Event object

The Event object describes an incoming [event](../../../core/events/).

## How to access it

* Use `event` in any task code
* Use `event` in the [Liquid console](../../../app/liquid-console.md) when viewing an event in Mechanic

## What it contains

* `event.topic` – containing the event topic (e.g. `shopify/customers/create`)
* `event.data` – containing all data that arrived with this event (e.g. the native webhook payload from Shopify, the payload after custom Shopify webhook customization, the data from an incoming email, etc)
* `event.source` – reflects the source that triggered the event (e.g. `"shopify"`, `"user"`, or `custom_shopify_webhook_subscription:<uuid>` for custom Shopify webhook deliveries)
* `event.created_at` – the date and time at which Mechanic received the event
* `event.shopify_topic` – on Shopify-sourced events (both native subscription deliveries and [custom Shopify webhook](../../shopify/custom-webhooks.md) deliveries), this contains the source Shopify topic in Mechanic's canonical form (e.g. `shopify/products/update`). For native deliveries it equals `event.topic`; for custom Shopify webhook deliveries, `event.topic` is the webhook's `user/...` topic and `event.shopify_topic` exposes the underlying Shopify topic. It's `nil` for non-Shopify events.
*   `event.parent` – if applicable, a reference to the event that used an [Event action](../../../core/actions/event.md) to create this event; parents are available up to five generations deep (e.g. `{{ event.parent.parent.parent.parent.parent }}`), but no further

    In preview mode, this object _also_ contains a "preview" attribute, as in `event.preview`, set to `true`. (In all other modes, event objects do _not_ have this property.) When this attribute is present, the task should render actions that are indicative of what the merchant should expect the task to do – and these "preview" actions will be shown to the merchant. These actions will also be used to determine what Shopify permissions Mechanic will request from the merchant. [Learn more about preview actions](../../../core/tasks/previews/)

## Storefront form customer

For a [cart form submission](../../webhooks.md#cart-contents-and-customer-context), `event.storefront_form_customer` verifies the customer context Shopify signed when it rendered the form. It returns a hash with `id`: a Shopify customer ID string for a signed-in visitor, or `nil` for a signed guest.

```liquid
{% assign form_customer = event.storefront_form_customer %}
{% if form_customer.id != nil %}
  {% assign customer_gid = form_customer.id | prepend: "gid://shopify/Customer/" %}
{% endif %}
```

A missing, invalid, altered, or expired proof on a form submission raises a Liquid error. An event without a form payload returns `nil`. Both Auto and Full request webhook modes are supported. Signature checks bind the shop, form, publication revision, webhook, customer, and render time. The proof must reach ingress within one hour; verification uses the event’s original receipt time, so queue delays and replaying that same event do not expire it.

This identifies the customer Shopify rendered. It does not authenticate the entered answers or cart contents, authorize order editing, or make a submission single use. Validate inputs needed by your task. Repeated HTTP submissions can create separate events and actions.

The helper is evaluated only when task code reads it, after normal webhook delivery. No additional verification request to Shopify or a special event topic is required. In task previews, use an illustrative customer fixture and render the actions needed for permission discovery, as usual.
