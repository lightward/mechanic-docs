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

## Thank you and Order status form request

For an [Thank you and Order status form](../../../app/thank-you-and-order-status-forms.md), `event.order_status_request` provides the request covered by Mechanic's signed receipt. The extension delivers it through the form's ordinary webhook topic. Use this object for the checked order context; a similarly named property in `event.data` is customer-supplied and is not trusted.

It includes the request ID, form ID/title/revision, order ID/name, customer ID, answer fields, selected line items, issue time and repeat policy. `placement` is `thank_you` or `order_status`. `verification.method` is `checkout` or `customer_account`, and `verification.customer_authenticated` states whether Mechanic verified a signed-in customer. Thank you can be a guest submission: the order-associated customer ID can be absent, and a present ID alone is not proof of customer authentication. Checkout tokens are not exposed in this object.

A missing or invalid receipt does not provide a trusted request; task code must require this object before acting on it. Receipts must reach the webhook within five minutes of authorization. Verification uses the original event receipt time, so a queue delay or rerunning that event does not expire an on-time receipt. The authorization describes that moment: a task making a consequential change must recheck its current business rules. Replaying an event can repeat actions; the receipt does not make delivery single use.
