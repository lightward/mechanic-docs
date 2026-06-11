---
description: >-
  Mechanic's Liquid objects — event, task, options, globals, secrets, cache, action, and Shopify resource objects available in your task code.
---

# Mechanic objects

Mechanic makes a variety of [**Liquid environment variables**](../../../core/tasks/code/environment-variables.md) available, containing specific Liquid objects. (The `shop` variable, for example, is always available and always contains the [Shop object](shopify/shop.md).)

Shop-level configuration is available through globals and secrets:

{% content-ref url="globals.md" %}
[globals.md](globals.md)
{% endcontent-ref %}

{% content-ref url="secrets.md" %}
[secrets.md](secrets.md)
{% endcontent-ref %}

Mechanic's Liquid objects mapping to Shopify resources all contain data pulled from the [Shopify's REST Admin API reference](https://shopify.dev/docs/admin-api/rest/reference). These objects are listed here:

{% content-ref url="shopify/" %}
[shopify](shopify/)
{% endcontent-ref %}

{% hint style="warning" %}
**Shopify variables in Mechanic do not necessarily contain the same attributes as Liquid variables used in Shopify (in places like themes or email templates) – even if they share the same name.**

In Mechanic, Shopify variables always contain data from Shopify events, which are delivered to Mechanic via webhook. This means that Shopify variables always have the same data structure as Shopify webhooks, corresponding to Shopify's REST representation for this data.

For example, while Shopify themes support `customer.name`, Mechanic does not (because [Shopify's REST representation of the customer resource](https://shopify.dev/docs/admin-api/rest/reference/customers/customer) does not contain a "name" property). On the other hand, Mechanic supports `customer.created_at`, while Shopify themes do not.
{% endhint %}
