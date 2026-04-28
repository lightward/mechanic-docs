---
description: >-
  Filter Shopify webhooks server-side, customize payloads with include_fields and metafields, and route deliveries onto a custom Mechanic event topic.
---

# Custom Shopify webhooks

A **custom Shopify webhook** is a Shopify webhook delivery, but reshaped — filtered, trimmed, and routed onto a Mechanic topic of your choosing — before it ever hits a task. Configure one in **Settings → Custom Shopify webhooks**.

A custom Shopify webhook can do four things native task subscriptions can't: send only the records you care about (filter), trim the payload (`include_fields`), include metafields alongside the resource (`metafield_namespaces`, `metafields`), and — for the first time in Mechanic — receive **metaobject** webhooks (`metaobjects/create`, `metaobjects/update`, `metaobjects/delete`). Each route delivers events onto a `user/...` topic of your choosing.

**Status legend** (full reference [below](#what-each-status-badge-means)): **Draft** · **Receiving** · **Not receiving** · **Needs permissions** · **Needs filter** · **Sync error** · **Disabled**.

## The 60-second version

Here's the smallest interesting example — a route that delivers `product_review` metaobject updates to a Mechanic topic, and a task that responds.

**Route config** (Settings → Custom Shopify webhooks → New):

* **Name** — `Product review updates`
* **Shopify topic** — `metaobjects/update`
* **Mechanic topic** — `user/metaobjects/product_review_update`
* **Filter** — `type:product_review`

**Task** with a subscription to `user/metaobjects/product_review_update` and this script:

```liquid
{% log
  metaobject_handle: event.data.handle,
  metaobject_type: event.data.type,
  fields: event.data.fields
%}
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

A few real moments these earn their keep:

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

**You need at least one of Filter / Include fields / Metafield namespaces / Metafields.** Without any of those, the route would be a duplicate of native delivery — so Mechanic gently declines.

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

Mechanic passes filters to Shopify verbatim. For the full grammar — including operators, supported fields per topic, wildcards, and quoting — see Shopify's [Customize webhook subscriptions: filters](https://shopify.dev/docs/apps/build/webhooks/customize/filters).

### Filter syntax preflight

Mechanic checks a few common syntax mistakes before saving and surfaces them as field errors:

* The filter has unmatched parentheses.
* The filter has unmatched quotes.
* The filter cannot end with `AND`, `OR`, or `NOT`.
* The filter has repeated logical operators.
* The filter has empty parentheses.

Anything past those checks goes to Shopify, which has the final say.

### Include fields must contain fields the filter references

If your filter references a field, that field also needs to be in `include_fields` (when `include_fields` is set). Otherwise Shopify can't evaluate the filter against the trimmed payload. Mechanic surfaces:

#### "Include fields must contain "{field}" because the filter references it."

Add the field to `include_fields` and save again. The check is prefix-matching, so `variants` in `include_fields` satisfies a filter referencing `variants.price` — list the parent field, not every leaf. (Without `include_fields` set, this rule doesn't apply: Shopify sees the full payload and can evaluate any filter field.)

### Metaobject topics require a `type:` filter

`metaobjects/create`, `metaobjects/update`, and `metaobjects/delete` only accept filters of the form `type:<handle>`. You can OR types together (`type:product_review OR type:faq_entry`), but `type:*` is rejected.

#### "Metaobject routes require a filter with type:..."

The route has no filter at all on a metaobject topic. Add one — e.g. `type:product_review`.

#### "Metaobject filters must include type:..."

A filter is set but doesn't include a `type:` clause. Add one.

#### "Metaobject filters cannot use type:\*"

The wildcard isn't supported on metaobject topics. List specific types instead.

### Field validation is best-effort

When the filter references fields, Mechanic checks them against Shopify's sample payload and surfaces a **warning** if a field can't be verified. This is preflight — Shopify is the final authority on save.

#### "Mechanic could not verify "{field}" for {topic} from Shopify's sample payload. Shopify will still validate the subscription on save."

If you're confident the field is valid (e.g. it exists in a Shopify API version newer than what the sample payload reflects), save anyway. Shopify will reject the subscription if the filter is actually invalid.

#### "Mechanic could not verify which fields this filter references from Shopify's sample payload. Shopify will still validate the subscription on save."

A different version of the same warning: the filter is non-blank but Mechanic couldn't parse any field references out of it (a filter like `id:*` triggers this). Same advice — Shopify is the authority on save.

### Verifying a filter

1. Save the route.
2. Trigger the source event in Shopify (create or update the matching resource).
3. Check the events list in Mechanic for a new event on the route's `user/...` topic.
4. Inspect `event.data` to confirm the payload arrived and matches expectations.

For end-to-end testing of routes, use a real Shopify event — task preview mode renders the task's Liquid against stub data, not against a synthetic route delivery, so it won't show you what Shopify will actually send through your filter and `include_fields` choices.

## Customizing the payload

Three fields control the payload Shopify delivers. Each is optional, and they can be combined.

* **`include_fields`** — top-level payload field whitelist. Example: `["id", "title", "variants"]`. The shape is determined by Shopify, not Mechanic — see Shopify's [modify-payloads docs](https://shopify.dev/docs/apps/build/webhooks/customize/modify-payloads) for what's included alongside the listed fields. The simplest way to be sure: configure your route, trigger a real event, and look at `event.data` on the resulting Mechanic event.
* **`metafield_namespaces`** — include all metafields in a namespace. Example: `["custom"]`. The metafields appear under `event.data.metafields`.
* **`metafields`** — include specific metafields by `namespace.key`. Example: `["custom.pack_size", "custom.lead_time_days"]`. Each entry must contain exactly one dot — `custom.foo.bar` is rejected ("must use namespace.key format, like \"custom.color\"").

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

This is the gotcha. Read it slowly the first time. Shopify treats a webhook subscription as unique by `(shop, app, destination, Shopify topic, filter)`. Mechanic's native task subscriptions on `shopify/...` topics are **unfiltered**. So an unfiltered custom route on the same Shopify topic collides with native delivery — Shopify will only keep one. (This applies whenever `filter` is blank, regardless of payload customization — `include_fields` / `metafields` / `metafield_namespaces` alone don't disambiguate.)

### Direction A: an unfiltered route lands while a task already has the native subscription

The route saves but stays in **Needs filter** state, with this banner:

#### "Without a filter, this custom webhook cannot receive alongside native {topic} task subscriptions. Add a filter, even id:\*, if you want this custom webhook to stay separate from native delivery."

(Where `{topic}` is the bare Shopify topic, e.g. `products/update` — this is what Shopify and Mechanic both use for native subscription matching.)

Add a filter to the route — even [`id:*`](#the-id-workaround) — to lift it out of this state.

### Direction B: enabling a task while an unfiltered route is active

If a custom route on `shopify/<topic>` is enabled and unfiltered, saving a task that subscribes to `shopify/<topic>` is blocked. The task editor's **Code** tab surfaces the conflict under "These Shopify subscriptions would stop unfiltered custom Shopify webhooks from receiving," followed by the per-route message:

#### "would stop {route name} from receiving its custom Shopify webhook. Add a filter to {route name}, disable it, or subscribe this task to {route's Mechanic topic} instead."

Three ways out: filter the route, disable the route, or migrate the task to subscribe to the route's `user/...` topic instead.

### The `id:*` workaround

`id:*` is an always-true filter — it matches every record, so it doesn't reduce delivery, but it counts as a non-empty filter for Shopify's uniqueness rule. Use it when you genuinely want a custom route to receive everything alongside native task subscriptions on the same topic. Yes, this looks ridiculous. Yes, it works — Mechanic itself recommends `id:*` in the **Needs filter** banner. (Metaobject topics are the exception — they require a `type:` filter, not `id:*`.)

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
4. If no other enabled task needs the unfiltered native delivery, the native subscription falls off automatically once no task uses it — but the next sync runs in a background job (within ~1 minute), and there's a brief window where both native and custom routes deliver. If your task is idempotent, you're fine; otherwise migrate during a quiet period or dedupe on the resource ID.
5. Verify required scopes still match in **Settings → Permissions** — auto-detection covers both task subscriptions and active routes.

To roll back: disable the route in **Settings → Custom Shopify webhooks** and revert the task subscription. The route's local config persists for re-enabling later.

## What it looks like in tasks

Four worked examples — the shapes you'll actually meet:

{% tabs %}
{% tab title="A · Metaobject delivery" %}

Route on `metaobjects/update` with filter `type:product_review`, delivering to `user/metaobjects/product_review_update`. Task subscribes to that Mechanic topic. The metaobject payload's `fields` is a hash keyed by field handle, not a list:

```liquid
{% log
  metaobject_id: event.data.id,
  metaobject_handle: event.data.handle,
  metaobject_type: event.data.type
%}

{% assign rating = event.data.fields["rating"] %}
{% assign body = event.data.fields["body"] %}

{% log rating: rating, body: body %}
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

**After** — task subscribed to `user/products/active-vip-update`, backed by a custom route with filter `status:active AND tags:VIP`. Same `event.data` shape (now narrower because of the filter), same script:

```liquid
{% log product_id: event.data.id %}
```

{% endtab %}
{% endtabs %}

`event.shopify_topic` carries the source Shopify topic in canonical Mechanic form (e.g. `shopify/products/update`) on every Shopify-sourced event — both native and custom-route deliveries. For native deliveries it equals `event.topic`; for custom-route deliveries `event.topic` is the route's `user/...` topic and `event.shopify_topic` is the original Shopify side. If you really need to detect the source in Liquid, `event.source` is `"shopify"` for native deliveries and `custom_shopify_webhook_subscription:<uuid>` for custom-route deliveries.

Subscription offsets work on `user/...` topics backed by routes the same way they do on native topics — e.g. `user/products/active-update+1.hour`. Monaco autocomplete in the task editor suggests `user/...` topics backed by enabled custom routes.

## If a custom route isn't behaving

Three ways this goes sideways, and how to recover from each:

**Stuck on Needs filter.** A native task subscription on the same Shopify topic is winning the uniqueness check. Add a filter to the route — see [When a custom route blocks native delivery](#when-a-custom-route-blocks-native-delivery), or use [`id:*`](#the-id-workaround) if you want the route to receive everything anyway.

**Status says Receiving but the task isn't running.** Confirm the task's subscription topic exactly matches the route's Mechanic topic — Monaco autocomplete suggests live route topics, which is the easiest way to avoid typos. Open an event on the events page and look for the **Custom Shopify webhook** provenance card to confirm the route delivered the event.

**Liquid can't see a field you expected.** Check the `include_fields` whitelist on the route — it's strict. If you're missing metafields specifically, see [`include_fields` doesn't select metafields](#include_fields-doesnt-select-metafields).

### Diagnosing delivery

When in doubt, work this checklist:

1. Is at least one **enabled** task subscribed to the route's `user/...` topic? (No subscribers means the route is healthy but not delivering — it'll show **Not receiving**.)
2. Is the Shopify subscription registered? EventBridge-backed subscriptions don't appear under Shopify Admin → Settings → Notifications → Webhooks — to confirm, run a `webhookSubscriptions(first: 250)` query in the Shopify Admin GraphiQL explorer and look for an entry named `mechanic_route_<32 hex chars>`.
3. Does the event detail page in Mechanic show the **Custom Shopify webhook** provenance card?
4. Trigger a real Shopify mutation (don't rely on preview mode for end-to-end testing).

Stuck after that? Paste the error or status into [Ask Mechanic](../../app/ask-mechanic.md) — it has full context on this feature, or ask in the [Mechanic Slack community](../../resources/slack.md).

## What each status badge means

Every route shows one of seven status badges. Row headers below match the badges verbatim — paste the badge text into search if you land here looking for one specific state.

| Status | Meaning | What to do |
|---|---|---|
| **Draft** | Unsaved new record. *Save this custom Shopify webhook to validate it with Shopify.* | Fill in the form and save. |
| **Receiving** | Enabled, in sync with Shopify, at least one task subscribed. *Shopify keeps this route synced.* | Nothing to do. |
| **Not receiving** | Enabled, but *no enabled tasks subscribe to the route's Mechanic topic yet*. | Subscribe a task to the route's Mechanic topic, or disable the route if it's no longer needed. |
| **Needs permissions** | *Saved in Mechanic, but inactive until Mechanic is granted the listed Shopify scope(s).* | Open **Settings → Permissions** (linked from the route) and grant the missing scopes. |
| **Needs filter** | An unfiltered route conflicts with a native task subscription on the same Shopify topic. *Add a filter before using this custom Shopify webhook with native task subscriptions.* | Add a filter — even [`id:*`](#the-id-workaround). See [When a custom route blocks native delivery](#when-a-custom-route-blocks-native-delivery). |
| **Sync error** | Shopify rejected the last sync attempt. *Saved in Mechanic, but Shopify is still using the last successful remote configuration if one exists.* | Read the inline detail and adjust the route. (When a disabled route fails cleanup: *Mechanic saved this custom Shopify webhook as disabled, but Shopify cleanup failed. The previous remote subscription may still exist.*) |
| **Disabled** | *This custom Shopify webhook is disabled. Shopify will not keep a remote subscription for it.* | Re-enable to resume delivery. |

## How routes stay in sync

A few behaviors that show up once you've been using this for a while:

* **Subscriber-gated.** A route stays subscribed to Shopify only while at least one *enabled* task subscribes to its Mechanic topic. With zero subscribers, the route shows **Not receiving** and the remote Shopify subscription is removed. The local route config persists.
* **Sync is async.** Sync runs in a background Sidekiq job (deduplicated for ~1 minute), so changes you make — disabling a route, subscribing a task, granting a scope — propagate to Shopify within seconds-to-a-minute, not synchronously.
* **Auto-reconciliation.** Subscribing a task to the route's Mechanic topic re-creates the remote Shopify subscription automatically (on the next sync run).
* **Permissions are auto-detected.** Routes contribute to Mechanic's required-scope set the same way task subscriptions do. The scope set is recomputed when a route is created, when its `enabled`, `event_topic`, or `shopify_topic` changes, or when the route is destroyed. A route in **Needs permissions** state lists the missing scopes inline with a link into Permissions.
* **Topic changes recreate the remote subscription, with a brief gap.** If you change a saved route's Shopify topic, Mechanic deletes the existing remote subscription before creating the new one. A Shopify event that fires in that window won't be delivered to either name.
* **Suspension and uninstall.** When the shop is suspended or uninstalled, Mechanic deletes all remote custom-route subscriptions; local config persists. On unsuspension or reinstall, routes re-sync automatically once tasks are subscribing.
* **Sync worker retries.** Shopify rate limits (429) and 5xx errors are retried with backoff. Authorization-style errors (401/402/403/404/423) are terminal — fix the underlying issue and re-save the route to trigger a fresh sync.
* **Don't edit routes in Shopify.** Each route registers a Shopify webhook subscription named `mechanic_route_<32 hex chars>` (the route's UUID with dashes removed). It's reachable via Shopify's GraphQL Admin API but not under Settings → Notifications → Webhooks, since these are EventBridge-backed. Manage them in Mechanic only — the sync worker owns reconciliation and will undo manual edits next time it runs.
* **Disabling a route stops future deliveries, not in-flight runs.** When you disable or delete a route, Mechanic deletes the remote Shopify subscription so no further deliveries arrive. Events Mechanic already received continue through the run queue normally — you'll see the last events finish even after the route is off.
* **Deleted routes degrade soft.** Historical events stay readable; the events list labels them as "Shopify route" or "(Unknown Shopify route)" rather than the original name.
* **Two routes can share a Mechanic topic.** Each route has its own Shopify subscription and creates events independently, so two routes with different filters routing to the same `user/...` topic will both fire — tasks subscribed to that topic see both deliveries. There's no deduplication. Mechanic surfaces a lint warning when you set this up, but it's allowed. Tell the deliveries apart by `event.source` — each route carries its own `custom_shopify_webhook_subscription:<uuid>` identifier.

## Import / export

Single routes can be exported and imported from a route's detail view. The list view supports importing a single route or an array of routes, and exporting selected routes.

* **List-view imports always land disabled.** Routes created via list-view import are explicitly disabled — they need review and a manual enable in the target shop.
* **Detail-view imports preserve the existing enabled state.** When you import into an existing route from its detail view, the new configuration replaces the old one but the route stays enabled if it was enabled, disabled if it was disabled.
* **Exact duplicates are skipped.**
* **Bulk import is sequential** — best for handfuls of routes at a time, not large catalogs.

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

On every Shopify-sourced event — both native and custom-route deliveries. The value is the source Shopify topic in Mechanic's canonical form (e.g. `shopify/products/update`). For native deliveries it equals `event.topic`; for custom-route deliveries `event.topic` is the route's `user/...` topic. If you need to detect the source in Liquid, `event.source` is `"shopify"` for native and `custom_shopify_webhook_subscription:<uuid>` for custom routes.

#### Why did enabling a task break my custom Shopify webhook?

Because the task subscribes to `shopify/<topic>` (unfiltered), and an unfiltered custom route on the same topic was already enabled. Shopify can't keep both. Add a filter to the route (even `id:*`), disable it, or migrate the task to subscribe to the route's Mechanic topic.

#### Can I have two routes deliver to the same `user/...` topic?

Yes. Each route has its own Shopify subscription and creates its own events; tasks subscribed to the shared topic see deliveries from both. Mechanic surfaces a lint warning when you set this up, but it's allowed. Tell deliveries apart by `event.source` — each route carries its own `custom_shopify_webhook_subscription:<uuid>` identifier.

## Related

* [Mechanic webhooks](../webhooks.md) — the inbound HTTP feature that shares the word "webhooks." Different feature, different direction.
* [How Shopify events reach Mechanic](../../core/shopify/events/README.md) — overview of native subscriptions vs. custom routes.
* [Event topics list](../events/topics.md) — every Shopify topic Mechanic supports natively.
* [Subscriptions](../../core/tasks/subscriptions.md) — how tasks listen for events.
* [Permissions](../../core/tasks/permissions.md) — how Mechanic auto-detects required Shopify scopes.
* [Event object](../liquid/objects/event.md) — where `event.shopify_topic` and the rest of the event surface are documented.
* Shopify: [Customize webhook subscriptions: filters](https://shopify.dev/docs/apps/build/webhooks/customize/filters) — the upstream authority on filter syntax.
* Shopify: [Customize webhook subscriptions: modify payloads](https://shopify.dev/docs/apps/build/webhooks/customize/modify-payloads) — the upstream authority on `include_fields` and metafield delivery.
