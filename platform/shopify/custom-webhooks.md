---
description: >-
  Filter Shopify webhooks server-side, customize payloads with include_fields and metafields, and route deliveries onto a custom Mechanic event topic.
---

# Custom Shopify webhooks

**Custom Shopify webhooks** let you reshape Shopify's own webhook deliveries — filter them, slim their payloads, route them to your own Mechanic topic — before they ever hit a task. Configure one in **Settings → Custom Shopify webhooks**.

A custom Shopify webhook can do four things native task subscriptions can't: send only the records you care about (filter), trim the payload (`include_fields`), include metafields alongside the resource (`metafield_namespaces`, `metafields`), and — for the first time in Mechanic — receive **metaobject** webhooks (`metaobjects/create`, `metaobjects/update`, `metaobjects/delete`). Each route delivers events onto a `user/...` topic of your choosing.

**Status legend** (full reference [below](#status-states)): **Draft** · **Receiving** · **Not receiving** · **Needs permissions** · **Needs filter** · **Sync error** · **Disabled**.

## The 60-second version

Here's the smallest interesting example — a route that delivers `product_review` metaobject updates to a Mechanic topic, and a task that responds.

**Route config** (Settings → Custom Shopify webhooks → New):

* **Name** — `Product review updates`
* **Shopify topic** — `metaobjects/update`
* **Mechanic topic** — `user/metaobjects/product_review_update`
* **Filter** — `type:product_review`

**Task** with a subscription to `user/metaobjects/product_review_update` and this script:

```liquid
{% log shopify_topic: event.shopify_topic %}
{% log data: event.data %}
```

Update a `product_review` metaobject in Shopify Admin and watch the event arrive. That's the shape. Read on for the full reference, or jump to [If a custom route isn't behaving](#if-a-custom-route-isnt-behaving) if you're already stuck.

## What this is — and what it isn't

Three concepts in Mechanic share the word "webhooks." Picking the right one matters.

| | Mechanic webhooks | Native Shopify subscriptions | Custom Shopify webhooks |
|---|---|---|---|
| **Direction** | Inbound HTTP from any system | Outbound from Shopify | Outbound from Shopify |
| **Triggers from** | Any HTTP client (forms, ERPs, CRMs, etc.) | Shopify | Shopify |
| **Event topic namespace** | `user/...` | `shopify/...` | `user/...` |
| **Filter at source** | n/a | No | Yes |
| **Payload customization** | n/a | No | Yes (`include_fields`, `metafield_namespaces`, `metafields`) |
| **Metaobject events** | n/a | No | Yes (with `type:` filter) |
| **Configured in** | Settings → Mechanic webhooks | Implicit (a task with `on: shopify/...`) | Settings → Custom Shopify webhooks |

If the data is coming from Shopify, you probably want a **Custom Shopify webhook** (this page) or a **native subscription** (just write a task with `on: shopify/<topic>`). If the data is coming from anything else — a form builder, a CRM, an ERP — you want a [Mechanic webhook](../webhooks.md) instead.

## When to reach for it

Concrete moments where a custom Shopify webhook earns its keep:

* **Stop firing tasks on draft orders you don't ship** — Filter `status:active` on `orders/updated`.
* **Get notified when a `product_review` metaobject changes** — `metaobjects/update` + Filter `type:product_review`. (Native subscriptions don't cover metaobject topics at all.)
* **Skip the 200 KB `products/update` payload when you only need price changes** — `include_fields: ["id", "variants"]`. (Mind the metafields gotcha [below](#customizing-the-payload).)
* **Receive a metafield value alongside its parent resource without a follow-up GraphQL call** — `metafields: ["custom.pack_size"]`.

## How to configure

Open **Settings → Custom Shopify webhooks** and click **New**. The form has these fields:

* **Name** — something to remember this route by.
* **Shopify topic** — pick from Mechanic's supported list (e.g. `products/update`, `orders/create`, `metaobjects/update`).
* **Mechanic topic** — the `user/...` topic events arrive on (e.g. `user/products/active-update`). Must match `user/[a-z0-9_]+/[a-z0-9_]+`.
* **Filter** — Shopify's webhook filter syntax. See [Filtering](#filtering).
* **Include fields** — top-level payload field whitelist.
* **Metafield namespaces** — include all metafields under a namespace.
* **Metafields** — include specific metafields by `namespace.key`.

**At least one of Filter / Include fields / Metafield namespaces / Metafields is required.** A route with no customization would just be a duplicate of native delivery, so Mechanic doesn't allow it.

### Try it

1. Open **Settings → Custom Shopify webhooks** → **New**.
2. Paste in the metaobject example from [The 60-second version](#the-60-second-version).
3. Save.
4. Create or edit a task that subscribes to `user/metaobjects/product_review_update`.
5. The route's status moves to **Receiving** once an enabled task subscribes. Update a `product_review` metaobject in Shopify Admin to confirm delivery.

## Filtering

The **Filter** field uses Shopify's webhook search syntax. Common patterns:

* **Equality** — `status:active`, `tags:VIP`, `vendor:"Nike Inc"` (quote values containing spaces).
* **Boolean** — `status:active AND tags:VIP`, `type:foo OR type:bar`, `NOT tags:test`.
* **Grouping** — `(status:active OR status:draft) AND tags:VIP`.
* **Always-true** — `id:*`. See [The `id:*` workaround](#the-id-workaround).

Mechanic passes filters to Shopify verbatim. For the full grammar — including the operators and fields supported per topic — see Shopify's [Customize webhook subscriptions: filters](https://shopify.dev/docs/apps/build/webhooks/customize/filters).

<!-- VERIFY: Confirm Shopify accepts `tags:VIP*` wildcards and bare numeric IDs (`id:1234567890`) before publishing. If either is wrong, remove the example. -->

### Metaobject topics require a `type:` filter

`metaobjects/create`, `metaobjects/update`, and `metaobjects/delete` only accept filters of the form `type:<handle>`. You can OR types together (`type:product_review OR type:faq_entry`), but `type:*` is rejected.

#### "Metaobject filters must include type:..."

The route is missing a `type:` filter on a metaobject topic. Add one — e.g. `type:product_review`.

#### "Metaobject filters cannot use type:\*"

The wildcard isn't supported on metaobject topics. List specific types instead.

### Field validation is best-effort

When the filter references fields, Mechanic checks them against Shopify's sample payload and surfaces a **warning** if a field can't be verified. This is preflight — Shopify is the final authority on save.

#### "Mechanic could not verify {field} for {topic} from Shopify's sample payload. Shopify will still validate the subscription on save."

If you're confident the field is valid (e.g. it exists in a Shopify API version newer than what the sample payload reflects), save anyway. Shopify will reject the subscription if the filter is actually invalid.

### Verifying a filter

1. Save the route.
2. Trigger the source event in Shopify (create or update the matching resource).
3. Check the events list in Mechanic for a new event on the route's `user/...` topic.
4. Inspect `event.data` to confirm the payload arrived and matches expectations.

Preview mode may not synthesize a sample for `user/...` topics backed by routes — test against a real Shopify event for the most accurate results.

<!-- VERIFY: Confirm whether Mechanic's preview generator synthesizes a sample for `user/...` topics backed by custom routes (and whether it reflects `include_fields`/`metafields`), or always falls back. Update the line above accordingly. -->

## Customizing the payload

Three fields control the payload Shopify delivers. Each is optional, and they can be combined.

* **`include_fields`** — top-level payload field whitelist. Example: `["id", "title", "variants"]`. Only the listed fields appear in `event.data`. <!-- VERIFY: Confirm whether `admin_graphql_api_id`, `id`, and any other always-present keys are still included alongside the listed fields, or strictly only the listed fields. Add a before/after sample payload tabs block once confirmed. -->
* **`metafield_namespaces`** — include all metafields in a namespace. Example: `["custom"]`. The metafields appear under `event.data.metafields`.
* **`metafields`** — include specific metafields by `namespace.key`. Example: `["custom.pack_size", "custom.lead_time_days"]`.

Shopify's authority on payload customization: [Customize webhook subscriptions: modify payloads](https://shopify.dev/docs/apps/build/webhooks/customize/modify-payloads).

### `include_fields` doesn't select metafields

This one trips nearly everyone the first time. **`include_fields` is a top-level payload field whitelist** — putting `custom.pack_size` in `include_fields` is treated as a regular payload path and won't include the metafield.

To deliver specific metafields *and* use `include_fields` to slim the rest of the payload:

1. Add the literal key `"metafields"` to `include_fields`.
2. List the entries you want in `metafields`.

Example: `include_fields: ["id", "title", "metafields"]` + `metafields: ["custom.pack_size"]`.

### Watch for Shopify API version changes

`include_fields` paths track Shopify's payload schema for the app's currently-subscribed API version. If Shopify changes a field name in a future API version, an `include_fields` route may deliver a different shape after the version bump. See [API versions](../../core/shopify/api-versions.md).

## When a custom route blocks native delivery

This is the gotcha worth understanding deeply. Shopify treats a webhook subscription as unique by `(shop, app, destination, Shopify topic, filter)`. Mechanic's native task subscriptions on `shopify/...` topics are **unfiltered**. So an unfiltered custom route on the same Shopify topic collides with native delivery — Shopify will only keep one.

### Direction A: an unfiltered route lands while a task already has the native subscription

The route saves but stays in **Needs filter** state, with this banner:

#### "Without a filter, this custom webhook cannot receive alongside native shopify/{topic} task subscriptions. Add a filter, even id:\*, if you want this custom webhook to stay separate from native delivery."

Add a filter to the route — even [`id:*`](#the-id-workaround) — to lift it out of this state.

### Direction B: enabling a task while an unfiltered route is active

If a custom route on `shopify/<topic>` is enabled and unfiltered, saving a task with `on: shopify/<topic>` is blocked at the API. The error names the conflicting route:

#### "would stop {route name} from receiving its custom Shopify webhook. Add a filter to {route name}, disable it, or subscribe this task to {route's Mechanic topic} instead."

Three ways out: filter the route, disable the route, or migrate the task to subscribe to the route's `user/...` topic instead.

### The `id:*` workaround

`id:*` is an always-true filter — it matches every record, so it doesn't reduce delivery, but it counts as a non-empty filter for Shopify's uniqueness rule. Use it when you genuinely want a custom route to receive everything alongside native task subscriptions on the same topic.

<!-- VERIFY: Confirm that Shopify accepts `id:*` on every supported topic. If certain topics reject it, document the exceptions. -->

### What's allowed

Filtered custom routes coexist with native subscriptions on the same topic just fine. Multiple filtered routes on the same topic are also allowed *as long as the filters differ* — Shopify uniqueness is `(topic, filter)`, so the same topic + same filter is the conflict. Two routes that share both topic and filter trigger:

#### "Another enabled custom Shopify webhook already uses this configuration."

#### "Shopify only allows one enabled custom Shopify webhook for each Shopify topic and filter."

### Payload customizations don't disambiguate

Two routes with the same Shopify topic + same filter but different `include_fields` or `metafields` still conflict. If you need two payload shapes for the same source, create one route and have multiple tasks subscribe to its Mechanic topic.

## Migrating a task from native to custom

Already have a task subscribed to `shopify/products/update` and want to move it onto a filtered custom route? The path:

1. Create the custom route with the desired filter and customization. Save.
2. Trigger the source event in Shopify and confirm the new `user/...` event appears in your events list.
3. Update the task's subscription from `shopify/<topic>` to the route's `user/...` topic.
4. If no other task needs the unfiltered native delivery, the native subscription falls off automatically once no enabled task uses it. Otherwise, the native and custom deliveries can coexist as long as the custom route is filtered.
5. Verify required scopes still match in **Settings → Permissions** — auto-detection covers both task subscriptions and active routes.

To roll back: disable the route in **Settings → Custom Shopify webhooks** and revert the task subscription. The route's local config persists for re-enabling later.

## What it looks like in tasks

Four worked examples covering the realistic shapes:

{% tabs %}
{% tab title="A · Metaobject delivery" %}

Route on `metaobjects/update` with filter `type:product_review`, delivering to `user/metaobjects/product_review_update`. Task subscribes to that Mechanic topic.

```liquid
{% log
  shopify_topic: event.shopify_topic,
  metaobject_handle: event.data.handle,
  fields: event.data.fields
%}
```

{% endtab %}

{% tab title="B · Filtered + slim payload" %}

Route on `products/update` with filter `status:active AND tags:VIP` and `include_fields: ["id", "variants"]`, delivering to `user/products/vip-update`. Task subscribes to that Mechanic topic.

```liquid
{% log
  shopify_topic: event.shopify_topic,
  product_id: event.data.id,
  variant_count: event.data.variants.size
%}
```

{% endtab %}

{% tab title="C · Metafields delivered" %}

Route on `products/update` with `metafield_namespaces: ["custom"]`, delivering to `user/products/with-metafields`. Task subscribes to that Mechanic topic and reads the metafield directly from the payload — no follow-up GraphQL call.

```liquid
{% assign pack_size = nil %}
{% for metafield in event.data.metafields %}
  {% if metafield.namespace == "custom" and metafield.key == "pack_size" %}
    {% assign pack_size = metafield.value %}
  {% endif %}
{% endfor %}

{% log product_id: event.data.id, pack_size: pack_size %}
```

{% endtab %}

{% tab title="D · Migrated subscription" %}

Same task script, before and after migrating from a native subscription to a filtered custom route.

**Before** — task subscribed to `shopify/products/update`, fires on every product update:

```liquid
{% log product_id: event.data.id %}
```

**After** — task subscribed to `user/products/active-vip-update`, backed by a custom route with filter `status:active AND tags:VIP`. Same data shape, plus `event.shopify_topic`:

```liquid
{% log
  product_id: event.data.id,
  shopify_topic: event.shopify_topic
%}
```

{% endtab %}
{% endtabs %}

A useful pattern: **`event.shopify_topic` is `nil` for events delivered via native subscriptions** and a string like `products/update` for events delivered via a custom route. So `{% if event.shopify_topic %}` is the canonical "is this a custom-route event?" check.

Subscription offsets work on `user/...` topics backed by routes the same way they do on native topics — e.g. `user/products/active-update+1.hour`. Monaco autocomplete in the task editor suggests `user/...` topics backed by enabled custom routes.

## If a custom route isn't behaving

Three failure modes, and how to recover from each:

**Stuck on Needs filter.** A native task subscription on the same Shopify topic is winning the uniqueness check. Add a filter to the route — see [When a custom route blocks native delivery](#when-a-custom-route-blocks-native-delivery), or use [`id:*`](#the-id-workaround) if you want the route to receive everything anyway.

**Status says Receiving but the task isn't running.** Confirm the task's subscription topic exactly matches the route's Mechanic topic — Monaco autocomplete suggests live route topics, which is the easiest way to avoid typos. Open an event on the events page and look for the **Custom Shopify webhook** provenance card to confirm the route delivered the event.

**Liquid can't see a field you expected.** Check the `include_fields` whitelist on the route — it's strict. If you're missing metafields specifically, see [`include_fields` doesn't select metafields](#include_fields-doesnt-select-metafields).

### Diagnosing delivery

When in doubt, work this checklist:

1. Is at least one **enabled** task subscribed to the route's `user/...` topic? (No subscribers means the route is healthy but not delivering — it'll show **Not receiving**.)
2. Is the Shopify subscription present in **Shopify Admin → Settings → Notifications → Webhooks** under the name `mechanic_route_{uuid}`?
3. Does the event detail page in Mechanic show the **Custom Shopify webhook** provenance card?
4. Trigger a real Shopify mutation (don't rely on preview mode for end-to-end testing).

Stuck after that? Paste the error or status into [Ask Mechanic](../../app/ask-mechanic.md) — it has full context on this feature, or ask in the [Mechanic Slack community](../../resources/slack.md).

## Status states

Every route shows one of seven status badges. Row headers below match the badges verbatim — paste the badge text into search if you land here looking for one specific state.

| Status | Meaning | What to do |
|---|---|---|
| **Draft** | Unsaved new record. | Save the route to validate it with Shopify. |
| **Receiving** | Enabled, in sync with Shopify, at least one task subscribed. | Nothing to do. |
| **Not receiving** | Enabled, but no enabled tasks subscribe to the route's Mechanic topic. | Subscribe a task, or disable the route if it's no longer needed. |
| **Needs permissions** | Saved, but a Shopify scope required by the topic hasn't been granted. | Open **Settings → Permissions** (linked from the route) and grant the missing scope(s). |
| **Needs filter** | Unfiltered route conflicts with a native task subscription on the same topic. | Add a filter — even [`id:*`](#the-id-workaround). See [When a custom route blocks native delivery](#when-a-custom-route-blocks-native-delivery). |
| **Sync error** | Shopify rejected the last sync attempt. | Read the inline detail and adjust. The previous remote subscription, if any, is still active. |
| **Disabled** | Local config retained; no Shopify subscription. | Re-enable to resume delivery. |

## Lifecycle and sync

A few behaviors worth knowing once you're using this feature regularly:

* **Subscriber-gated.** A route stays subscribed to Shopify only while at least one *enabled* task subscribes to its Mechanic topic. With zero subscribers, the route shows **Not receiving** and the remote Shopify subscription is removed. The local route config persists.
* **Auto-reconciliation.** Subscribing a task to the route's Mechanic topic re-creates the remote Shopify subscription automatically.
* **Permissions are auto-detected.** Routes contribute to Mechanic's required-scope set, the same way task subscriptions do. <!-- VERIFY: Document the exact triggers for scope recomputation (route save, task save, deploy, etc.). --> A route in **Needs permissions** state lists missing scopes inline with a link into Permissions.
* **Don't edit routes in Shopify Admin.** Routes appear in the merchant's Shopify Admin webhook list as `mechanic_route_{uuid}`. Manage them in Mechanic only — Mechanic owns reconciliation and will undo manual edits.
* **Deleted routes degrade soft.** If a route is deleted after delivering events, those historical events stay readable; the source provenance card just disappears from event detail.

<!-- VERIFY: Document what happens to in-flight runs (event runs / task runs / action runs already enqueued) when a route is disabled or deleted. Do they complete or get cancelled? -->

<!-- VERIFY: When two distinct routes (different filters or customization) both deliver to the same `user/...` topic, do both deliveries arrive at subscribed tasks? Are they distinguishable beyond `event.shopify_topic`? -->

## Import / export

Single routes can be exported and imported from a route's detail view. The list view supports importing a single route or an array of routes, and exporting selected routes.

* **Imports always land disabled.** Imported routes need explicit review and enable in the target shop.
* **Exact duplicates are skipped.**
* **Bulk import is sequential** — best for handfuls of routes at a time, not large catalogs.

<!-- VERIFY: Confirm that Shopify accepts a route configured with *only* `metafield_namespaces` (no filter, no `include_fields`), or whether some topics require additional customization. The "at least one of four is enough" claim above depends on this. -->

## Frequently asked

#### Why isn't my custom Shopify webhook receiving?

Three usual culprits: no *enabled* task subscribes to the route's Mechanic topic (the status will show **Not receiving**); a Shopify scope is missing (**Needs permissions**); or the route is colliding with a native task subscription on the same topic (**Needs filter**). Walk through [If a custom route isn't behaving](#if-a-custom-route-isnt-behaving) for the full diagnosis.

#### What does `id:*` mean in a Shopify webhook filter?

It's an always-true filter — it matches every record, so it doesn't reduce delivery, but it counts as a non-empty filter for Shopify's uniqueness rule. Use it when you want a custom route to receive everything alongside native task subscriptions on the same Shopify topic.

#### Can I get metaobject webhooks in Mechanic?

Yes — through custom Shopify webhooks. Native Mechanic subscriptions don't cover `metaobjects/create`, `metaobjects/update`, or `metaobjects/delete`, but custom Shopify webhooks do, with a required `type:` filter (e.g. `type:product_review`).

#### What's the difference between `include_fields` and `metafields`?

`include_fields` is a top-level payload field whitelist; it does *not* select metafields. To deliver specific metafields *and* use `include_fields`, add the literal key `"metafields"` to `include_fields` and list the entries in `metafields`.

#### Where is `event.shopify_topic` set?

On every event delivered via a custom Shopify webhook route. It contains the original Shopify topic (e.g. `products/update`). It's `nil` on events from native task subscriptions, so `{% if event.shopify_topic %}` is the canonical "is this from a custom route?" check.

#### Why did enabling my task break a custom Shopify webhook?

Because the task subscribes to `shopify/<topic>` (unfiltered), and an unfiltered custom route on the same topic was already enabled. Shopify can't keep both. Add a filter to the route (even `id:*`), disable it, or migrate the task to subscribe to the route's Mechanic topic.

## Related

* [Mechanic webhooks](../webhooks.md) — the inbound HTTP feature that shares the word "webhooks." Different feature, different direction.
* [How Shopify events reach Mechanic](../../core/shopify/events/README.md) — overview of native subscriptions vs. custom routes.
* [Event topics list](../events/topics.md) — every Shopify topic Mechanic supports natively.
* [Subscriptions](../../core/tasks/subscriptions.md) — how tasks listen for events.
* [Permissions](../../core/tasks/permissions.md) — how Mechanic auto-detects required Shopify scopes.
* [Event object](../liquid/objects/event.md) — where `event.shopify_topic` and the rest of the event surface are documented.
* Shopify: [Customize webhook subscriptions: filters](https://shopify.dev/docs/apps/build/webhooks/customize/filters) — the upstream authority on filter syntax.
* Shopify: [Customize webhook subscriptions: modify payloads](https://shopify.dev/docs/apps/build/webhooks/customize/modify-payloads) — the upstream authority on `include_fields` and metafield delivery.
