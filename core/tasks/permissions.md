# Permissions

Mechanic determines Shopify API access scopes (like `read_orders` or `write_products`) from your task code. It can infer scopes from several places, including subscriptions, Shopify data access in task code, and actions rendered during preview. When your shop needs additional access, Mechanic will surface that in the app.

There are two approaches, and both are fully supported:

## Automatic detection

By default, Mechanic infers the permissions a task needs automatically.

This can happen through:

* Shopify event subscriptions
* Shopify data your task reads in Liquid
* Shopify actions your task generates during [preview](previews/)

Previews are valuable for more than permissions: they show users what the task will do, and they give developers a safe way to verify task behavior. They are still especially important when permission requirements depend on action arguments. For example, mutations like `tagsAdd` or `metafieldsSet` may require different scopes depending on the resource ID used, so realistic preview data still matters.

To get the best results:

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

This tells Mechanic precisely which scopes to request, without relying on automatic detection. It's simpler and more predictable — useful when:

* Your task's permissions are hard to infer automatically
* You want to be explicit about what your task needs
* Your task uses scopes that aren't easily demonstrated during preview

For full syntax details, see the [`permissions` tag reference](../../platform/liquid/tags/permissions.md).

## When to use which

**Automatic detection** works well for many tasks out of the box, especially when the task's subscriptions, Shopify data access, and previewed actions clearly demonstrate its intent.

**Explicit declaration** is simpler when permissions are hard to infer, when Mechanic fails to detect a permission your task needs, or when setting up realistic previews would be more work than just listing the scopes directly.

Both approaches are valid, and you can choose whichever fits your task best.

## Troubleshooting

If your task is failing because of a missing permission, see [My task is failing because of a permissions problem](../../faq/my-task-is-failing-because-of-a-permissions-problem.md) for debugging steps. For general questions about managing Shopify scopes, see [How do I add a Shopify access scope to my task?](../../faq/how-do-i-add-a-shopify-access-scope-to-my-task.md)
