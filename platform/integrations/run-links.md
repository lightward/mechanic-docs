---
description: Mechanic run links give your users a useful point of entry.
---

# Run links

Some tasks lend themselves to manual invocation by a user, on demand. In these cases, Mechanic makes available **run links**: URLs from which a task run may be created, suitable for bookmarking or sharing.

Run links can be useful in an external application, email, or website – anywhere a custom user experience would be improved by a shortcut to an opportunity to craft a specific Mechanic task run.

{% hint style="info" %}
[Shopify admin action links](../../core/shopify/admin-action-links.md) are a form of run link.
{% endhint %}

## Format

Run link format varies based on the style of task run desired.

### Manual trigger

For tasks subscribing to mechanic/user/trigger and mechanic/user/text, the run link points to /apps/mechanic/tasks/{task id}/run, as illustrated below:

```
https://example.myshopify.com/admin/apps/mechanic/tasks/7ec...da4/run
```

For tasks subscribing to mechanic/user/text, the text submission may be prefilled using the `data` query parameter, as in `?data=drink+me%21`.

### Shopify resources

For tasks subscribing to mechanic/user/{Shopify resource} (i.e. for tasks responding to [Shopify admin action links](../../core/shopify/admin-action-links.md)), create a link pointing to /apps/mechanic/tasks/run within the Shopify admin, using the `resource_type` and `ids[]` query parameters as shown below.

```
https://example.myshopify.com/admin/apps/mechanic/tasks/run?resource_type=order&ids[]=12345&ids[]=67890
```

Breaking this down, the URL consists of the following:

*   Protocol, host, path

    ```html
    https://example.myshopify.com/admin/apps/mechanic/tasks/run
    ```
*   `resource_type` query parameter

    ```html
    resource_type=order
    ```
*   `ids[]` query parameter

    ```html
    ids[]=12345&ids[]=67890
    ```

Repeat the `ids[]` query parameter as needed, for each relevant resource ID.
