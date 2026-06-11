---
description: Reference write-only shop secrets from Liquid without exposing raw values.
---

# Secrets object

The `secrets` object contains write-only shop-level secrets from [Globals and secrets](../../globals-and-secrets.md).

Use secrets for credentials and signing material:

```liquid
{{ secrets.api_token }}
```

If the secret exists, this returns an opaque secret reference, not the raw secret value. Mechanic turns that reference into the real value only inside supported actions and filters, such as HTTP actions, specific FTP connection fields, and signing filters like `hmac_sha256` or `rsa_sha256`.

Missing secrets return `nil`, so guards like `{% if secrets.api_token %}` work as expected.

Actions that do not support shop secrets, logs, and ordinary Liquid output receive the placeholder. This lets tasks pass references around without exposing secret values in previews, stored action options, logs, or task output.

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
