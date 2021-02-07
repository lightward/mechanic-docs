# Subscriptions

A task **subscription** is the expression of a task's intent to receive certain [**events**](../events/), filtering by [**topic**](../events/event-topic-reference/). A subscription consists of an event topic, optionally combined with an **offset**. A task may have any number of subscriptions.

## Offsets

Offsets are used to specify a length of time that the task should wait before responding to the event.

Subscription offsets are appended to the subscription topic, and are of the form "+1.hour". Offsets may be given using minutes, hours, days, weeks, months, or years.

A subscription consisting of an event topic and an offset looks like "shopify/customers/create+1.hour".

{% hint style="warning" %}
The [Shopify variables](code/liquid-variables.md#shopify-variables) available to tasks always contain data drawn from the event itself. If a task has a offset event subscription, this data may be outdated by the time the task runs.

To reload the data in a Shopify variable, use something like this:

```javascript
{% unless event.preview %}
  {% assign customer = customer.reload %}
{% endunless %}
```

Remember, Mechanic does not permit access to the Shopify API during [event preview](previews.md). Using `{% unless event.preview %}`ensures that reloading only happens during a live event.
{% endhint %}

## Using Liquid

A task's subscriptions are parsed for Liquid, at the time the task is saved. Combined with [**task options**](options.md), this is an opportunity to generate subscriptions based on user configuration, adding or removing subscriptions based on the user's choice, or adjusting subscription offset based on a user-entered value.

One subscription is permitted per line. Blank lines and leading/trailing whitespace is permitted.

### Examples

{% tabs %}
{% tab title="Conditional subscription" %}
```javascript
shopify/orders/create

{% if options.send_email_when_order_cancelled__boolean %}
  shopify/orders/cancelled
{% endif %}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Dynamic offset" %}
```javascript
shopify/orders/paid+{{ options.days_to_wait_before_followup__number_required }}.days
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Optional offset" %}
```javascript
shopify/customers/create{% if options.wait_one_hour__boolean %}+1.hour{% endif %}
```
{% endtab %}
{% endtabs %}

