# Permissions

Mechanic requests Shopify API access scopes (like `read_orders` or `write_products`) on behalf of your tasks. When a task needs a new scope, Mechanic prompts you to approve it. This page explains how Mechanic determines which scopes a task needs.

There are two approaches, and both are fully supported:

## Automatic detection from previews

By default, Mechanic infers the permissions a task needs by analyzing the actions and GraphQL queries it generates during [preview](previews/). For example, if a task renders a Shopify action containing a `customerCreate` mutation during preview, Mechanic will request the `write_customers` scope.

This approach works well when your task's previews already generate realistic actions. To get the best results:

* Make sure your preview actions include realistic resource IDs (e.g. `gid://shopify/Product/12345`), especially for mutations like `tagsAdd` or `metafieldsSet` that have multiple potential scope requirements.
* Use [defined preview events](previews/events.md) and [stub data](previews/stub-data.md) to provide your task with realistic sample data during preview.

For full details on how previews work, see [Previews](previews/).

## Explicit declaration with `{% permissions %}`

You can declare the exact scopes your task needs directly in your task code using the `{% permissions %}` tag:

```liquid
{% permissions %}
read_orders
write_products
{% endpermissions %}
```

This tells Mechanic precisely which scopes to request, without relying on preview analysis. It's simpler and more predictable — useful when:

* Your task's permissions are hard to infer from previews alone
* You want to be explicit about what your task needs
* Your task uses scopes that aren't easily demonstrated during preview

For full syntax details, see the [`permissions` tag reference](../../platform/liquid/tags/permissions.md).

## When to use which

**Automatic detection** works well when your task previews already generate realistic actions that fully demonstrate the task's intent. Many tasks work this way out of the box, especially simpler tasks with straightforward Shopify mutations.

**Explicit declaration** is simpler when permissions are hard to infer, when Mechanic fails to auto-detect a permission your task needs, or when setting up realistic previews would be more work than just listing the scopes directly.

Both approaches are valid, and you can choose whichever fits your task best.

## Troubleshooting

If your task is failing because of a missing permission, see [My task is failing because of a permissions problem](../../faq/my-task-is-failing-because-of-a-permissions-problem.md) for debugging steps. For general questions about managing Shopify scopes, see [How do I add a Shopify access scope to my task?](../../faq/how-do-i-add-a-shopify-access-scope-to-my-task.md)
