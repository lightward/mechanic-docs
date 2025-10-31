# User Form

When a task subscribes to the **mechanic/user/form** event topic a "Run task" button is added to the task.

When the Run Task button is clicked the user is presented with a form that contains any [task options](options/) that have the `_userform` flag.&#x20;

When submitted, an event is generated, to which only this task will respond. The event contains the user's input in its data, making user's input available in `event.data.`

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>User form on Run Task page</p></figcaption></figure>

{% hint style="info" %}
Click the link button beside the title of the form to copy a link to the form that you can share with your users
{% endhint %}

### Getting the user’s input in code

During a `mechanic/user/form` event, the ad-hoc values arrive under event.data.

```liquid
{% # These will appear on the run task user form %}
{% assign big_event = options.the_big_event__date_userform %}
{% assign color_for_big_event = options.color_for_big_event__color_required_userform %}

{% # This will NOT appear on the run task user form %}
{% assign level = options.level__select_o1_low_o2_high %}

{% if event.topic == "mechanic/user/form" %}
  {% # we need to get the value from event data, we don't want the value from the task option %}
  {% assign big_event = event.data.the_big_event__date_userform %}
  {% assign color_for_big_event = event.data.color_for_big_event__color_required_userform %}

{% endif %}

{% action "echo" big_event, level, color_for_big_event %}
```

{% hint style="info" %}
Tip: `event.data` contains the values for the fields you exposed; fall back to\
`options.*` for everything else.
{% endhint %}
