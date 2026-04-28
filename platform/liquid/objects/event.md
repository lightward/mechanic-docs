# Event object

The Event object describes an incoming [event](../../../core/events/).

## How to access it

* Use `event` in any task code
* Use `event` in the [Liquid console](../../../app/liquid-console.md) when viewing an event in Mechanic

## What it contains

* `event.topic` – containing the event topic (e.g. `shopify/customers/create`)
* `event.data` – containing all data that arrived with this event (e.g. the webhook payload from Shopify, the data from an incoming email, etc)
* `event.source` – reflects the entity that triggered the event (e.g. `"shopify"`, `"user"`)
* `event.created_at` – the date and time at which Mechanic received the event
* `event.shopify_topic` – when the event was delivered via a [Custom Shopify webhook](../../shopify/custom-webhooks.md), this contains the original Shopify topic (e.g. `products/update`). It's `nil` for events from native Shopify subscriptions and other sources, so `{% if event.shopify_topic %}` is the canonical "is this from a custom route?" check.
*   `event.parent` – if applicable, a reference to the event that used an [Event action](../../../core/actions/event.md) to create this event; parents are available up to five generations deep (e.g. `{{ event.parent.parent.parent.parent.parent }}`), but no further

    In preview mode, this object _also_ contains a "preview" attribute, as in `event.preview`, set to `true`. (In all other modes, event objects do _not_ have this property.) When this attribute is present, the task should render actions that are indicative of what the merchant should expect the task to do – and these "preview" actions will be shown to the merchant. These actions will also be used to determine what Shopify permissions Mechanic will request from the merchant. [Learn more about preview actions](../../../core/tasks/previews/)
