# Can I manually set Shopify permissions for Mechanic?

Mechanic offers two ways to manage Shopify permissions for your tasks:

1. **Automatic detection from previews** — Mechanic analyzes the actions your task generates during [preview](../core/tasks/previews/) to infer which scopes it needs.
2. **Explicit declaration with `{% permissions %}`** — You can declare the exact scopes your task needs directly in your [task code](../platform/liquid/tags/permissions.md).

Both approaches are fully supported. See [Permissions](../core/tasks/permissions.md) for a full overview.

{% hint style="info" %}
Having trouble getting your permissions and previews to line up? Try asking in our [community Slack workspace](../resources/slack.md). Sharing your task code there is a good way to get suggestions about how to move forward. :)
{% endhint %}

