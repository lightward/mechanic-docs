# Writing a high-quality task

Here are some best practices we've found, as a community, about writing tasks that improve life for everyone.

{% hint style="info" %}
Working on getting better at task-writing? See [Practicing writing tasks](../resources/tutorials/practicing-writing-tasks.md).
{% endhint %}

* [**Know how Mechanic uses Liquid.**](../platform/liquid/) We've extended the language with new tags and filters – being familiar with them will help you write a task that works efficiently, and reads cleanly.
* [**Render preview actions.**](../core/tasks/previews/) Do so by adding stub data to the top of your task script, allowing your task to render those actions in exactly the same way it would for a real event.
* [**Watch out for action loops.**](https://docs.usemechanic.com/article/453-avoiding-action-loops) If your task modifies a Shopify resource and also subscribes to that resource's update event, ensure that the task knows to not attempt to update it a second time.
* **If you generate actions that destroy data,** [**log the original values**](../platform/liquid/tags/log.md)**.** You never know when you'll need to go back and pull the data that was in place before your action ran.
* **If your task sometimes does nothing,** [**log the reason why**](../platform/liquid/tags/log.md)**.** This simple practice can save hours of customer support time later.
* **When you want to do work** _**x**_ **days after** _**y**_**, choose purposefully between delayed subscriptions and regular batch processing.** Instead of using "shopify/orders/create+5.days" to queue up a bunch of future work, consider a task that runs on "mechanic/scheduler/daily" which queries for all orders that were created 5 days ago, processing all of them as a batch. This practice allows the merchant to disable/enable the task at will, without having to think about events that are already scheduled, and without having to wait _x_ days before the task starts working again.
* **Recall that Shopify** _**technically**_ **doesn't guarantee webhook deliverability.** We've actually never seen this be a problem – sometimes they're delayed, but we haven't seen them go missing. So, write tasks that can handle being run a few hours late.
* **Include a test mode that renders a descriptive "echo" action.** This action should return a summary of what the task _would_ actually do if test mode were disabled. Use a boolean – something like `{% if options.test_mode__boolean %}`. Particularly for tasks that have the potential to change a lot of data at once, this allows the merchant to make an informed decision about what happens next.

