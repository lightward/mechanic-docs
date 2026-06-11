---
description: Read visible shop-level globals from Liquid.
---

# Globals object

The `globals` object contains visible shop-level configuration from [Globals and secrets](../../globals-and-secrets.md).

Use globals for values that are safe to display, export, and sync with task repos:

```liquid
{{ globals.account_username }}
```

Globals may contain any JSON value. If a global stores structured JSON, use normal Liquid lookups and loops:

```liquid
{% for region in globals.shipping_rules.regions %}
  {{ region }}
{% endfor %}
```

If a key does not exist, `globals.some_key` returns `nil`.

Task options can also let a merchant choose a global:

```liquid
{{ options.shared_username__global_required }}
```

That option returns the selected global's value at runtime. See [Task options](../../../core/tasks/options/) for option flag details.
