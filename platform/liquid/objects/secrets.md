---
description: Reference write-only shop secrets from Liquid without exposing raw values.
---

# Secrets object

The `secrets` object contains write-only shop-level secrets from [Globals and secrets](../../globals-and-secrets.md).

Use secrets for credentials and signing material:

```liquid
{{ secrets.api_token }}
```

This returns an opaque secret reference, not the raw secret value. The raw value is only resolved by supported secret-aware paths, such as HTTP actions, FTP actions, and HMAC or related digest filters.

Unsupported actions, logs, and ordinary Liquid output receive the placeholder. This lets tasks pass references around without exposing secret values in previews, stored action options, logs, or task output.

For example, this HTTP action sends the resolved value in the outbound request, while Mechanic stores and displays only placeholders or redacted values:

```liquid
{% action "http" %}
  {
    "method": "post",
    "url": "https://api.example.com/orders",
    "headers": {
      "Authorization": {{ "Bearer " | append: secrets.api_token | json }}
    }
  }
{% endaction %}
```

Task options can also let a merchant choose a secret:

```liquid
{{ options.api_token__secret_required }}
```

That option returns the selected secret reference at runtime. See [Task options](../../../core/tasks/options/) for option flag details.
