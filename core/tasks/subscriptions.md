# Subscriptions

A task **subscription** is the expression of a task's intent to receive certain [**events**](../events/), filtering by [**topic**](../events/topics.md). A subscription consists of an event topic, optionally combined with a time **offset**, which creates a delay.

A task may have any number of subscriptions.

![](<../../.gitbook/assets/Screen Shot 2022-05-05 at 10.09.07 AM.png>)

## Delays

... are accomplished using subscription offsets, as described below. This heading is here for folks searching for a way to delay their tasks. ;)

## Offsets

A subscription offset (sometimes called a delay) defines the amount of time a task should wait or delay (!!) before responding to the incoming event. It's the easiest way to add a delay to a task's subscription to a specific topic. (For finer control over event timing, try using the `run_at` option of the [Event action](../actions/event.md).)

Subscription offsets are appended to the subscription topic, and are of the form "+1.hour". Offsets may be given using seconds, minutes, hours, days, weeks, months, or years. There is no limit to how large the subscription offset may be.

**A subscription with an offset looks like `shopify/customers/create+1.hour`.**

To learn more about scheduling work with Mechanic, see [Scheduling](../runs/scheduling.md).

{% hint style="info" %}
In practice, large offsets can make debugging difficult in practice! If you're thinking about work to be done weeks or months or years from now, consider running an hourly or daily task that scans for work that's due to be done, instead of scheduling tasks for the distant future.
{% endhint %}

{% hint style="warning" %}
The [Shopify variables](code/environment-variables.md#shopify-variables) available to tasks always contain data drawn from the event itself. If a task has a offset event subscription, this data may be outdated by the time the task runs.

To reload the data in a Shopify variable, use something like this:

```liquid
{% raw %}
{% unless event.preview %}
  {% assign customer = customer.reload %}
{% endunless %}
{% endraw %}
```

Remember, Mechanic does not permit access to the Shopify API during [event preview](previews/). Using this `unless` statement ensures that reloading only happens during a live event.
{% endhint %}

## Using Liquid

A task's subscriptions are parsed for Liquid, at the time the task is saved. Combined with [**task options**](options/), this is an opportunity to generate subscriptions based on user configuration, adding or removing subscriptions based on the user's choice, or adjusting subscription offset based on a user-entered value.

One subscription is permitted per line. Blank lines and leading/trailing whitespace are permitted.

### Examples

{% tabs %}
{% tab title="Conditional subscription" %}
```liquid
shopify/orders/create

{% raw %}
{% if options.send_email_when_order_cancelled__boolean %}
  shopify/orders/cancelled
{% endif %}
{% endraw %}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Dynamic offset" %}
```liquid
shopify/orders/paid+{{ options.days_to_wait_before_followup__number_required }}.days
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Optional offset" %}
```liquid
shopify/customers/create{% raw %}
{% if options.wait_one_hour__boolean %}+1.hour{% endif %}
{% endraw %}
```
{% endtab %}
{% endtabs %}
