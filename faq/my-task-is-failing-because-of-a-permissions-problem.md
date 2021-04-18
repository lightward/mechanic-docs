# My task is failing because of a permissions problem. Why?

If you're creating a new task, and writing a script to go with it, you might see an error like this when you start running it for the first time:

```text
Received a Shopify error (403): {"errors":"[API] This action requires merchant approval for write_orders scope."}
```

Or, if you're using GraphQL, you might see something like this:

```text
"message": "TagsAdd access denied"
```

Mechanic works hard to detect necessary permissions, and prompt you for them up front. If you see an error message like this one, it means that Mechanic wasn't able to detect everything necessary ahead of time.

This issue can almost always be solved by generating the right preview actions, demonstrating to Mechanic what exactly you intend to do.

{% page-ref page="../core/tasks/previews/" %}

