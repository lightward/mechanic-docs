---
description: "Write Shopify automations in Liquid with Mechanic's custom tags, filters, and objects — including inline GraphQL queries and action definitions."
---

# Liquid Scripting in Mechanic

Everything in Mechanic is about [**tasks**](../../core/tasks/), and tasks are written in **Liquid** — the template language created by Shopify. But Mechanic extends Liquid into a scripting environment: tasks can query the Shopify Admin API inline, loop through paginated results, transform data, and render complex action sequences — all in one script.

Fundamentally, [**task code**](../../core/tasks/code/) renders JSON instructions for [**actions**](../../core/actions/). In the same way that a Liquid theme takes store data and renders HTML, Mechanic task code takes incoming [**events**](../../core/events/) and uses them to render JSON objects defining work to be performed. The difference is that Mechanic Liquid has the control flow and data manipulation capabilities needed for real automation logic.

## What does this look like?

Here's a simple task that tags a customer when they place their first order:

```liquid
{% if event.preview %}
  {% assign customer = hash %}
  {% assign customer["admin_graphql_api_id"] = "gid://shopify/Customer/1234567890" %}
  {% assign customer["orders_count"] = 1 %}
{% endif %}

{% if customer.orders_count == 1 %}
  {% action "shopify" %}
    mutation {
      tagsAdd(id: {{ customer.admin_graphql_api_id | json }}, tags: ["new-customer"]) {
        userErrors { field message }
      }
    }
  {% endaction %}
{% endif %}
```

The task starts with a [preview block](../../core/tasks/previews/) that provides stub data — this lets Mechanic know what permissions the task needs and shows you what the task will do before it runs with real data. Then it reads data from the incoming event (`customer.orders_count`), makes a decision using standard Liquid logic (`{% if %}`), and outputs an action (`{% action "shopify" %}`) containing a GraphQL mutation. This is the core pattern: events come in, Liquid processes them, actions go out.

## Mechanic Liquid vs. Shopify theme Liquid

If you've worked with Shopify themes, Mechanic Liquid will feel familiar — but the context is different. Theme Liquid renders HTML for a storefront. Mechanic Liquid renders JSON actions in response to events. The available objects are different too: instead of `product` and `collection`, you'll work with [`event`](objects/event.md), [`task`](objects/), and [`options`](objects/options.md). And Mechanic adds its own tags — like [`{% action %}`](tags/action.md), [`{% log %}`](tags/log.md), and [`{% error %}`](tags/error.md) — that have no equivalent in themes.

## One Liquid, many implementations

**Open-source Liquid** is a template language created by Shopify, used by many developers for many projects and products in, around, and beyond Shopify. ([Learn the basics here!](basics/))

**Shopify Liquid** is the implementation of open-source Liquid used within Shopify itself, including Shopify's Online Store themes. Shopify Liquid contains many filters and objects that are not a part of open-source Liquid.

**Mechanic Liquid** is the implementation of open-source Liquid used here, within Mechanic. It contains many filters and objects that are unique to Mechanic, in addition to some unique language features.

* [**Mechanic filters**](filters/) make it easier to manage data in the context of Mechanic tasks. Mechanic Liquid also includes support for many Shopify Liquid filters!
* [**Mechanic keyword literals**](keyword-literals/) create support for [arrays](keyword-literals/array.md) and [hashes](keyword-literals/hash.md).
* [**Mechanic objects**](objects/) represent specific Mechanic and Shopify resources.
* [**Mechanic tags**](tags/) make it easier for task writers to render JSON objects. These tags include [action](tags/action.md), [log](tags/log.md), and [error](tags/error.md). We've also enhanced the [assign](tags/assign.md) tag to support assigning values within arrays and hashes.

{% hint style="info" %}
Want to experiment? The [Liquid console](../../app/liquid-console.md) is a live scratchpad for testing Mechanic Liquid, available in the footer of every page of the app.
{% endhint %}
