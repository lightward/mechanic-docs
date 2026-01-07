# User Form

When a task subscribes to the **mechanic/user/form** event topic a "Run task" button is added to the task.

When the Run Task button is clicked the user is presented with a form that contains any [task options](options/) that have the `_userform` flag.&#x20;

When submitted, an event is generated, to which only this task will respond. The user's input is available in Liquid as `input.<name>`.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>User form on Run Task page</p></figcaption></figure>

{% hint style="info" %}
Click the link button beside the title of the form to copy a link to the form that you can share with your users
{% endhint %}

### Getting the user’s input in code

During a `mechanic/user/form` event, ad-hoc values are available as `input.<name>`. `event.data` also contains these values for backward compatibility.

```liquid
{% # These will appear on the run task user form %}
{% assign big_event = options.the_big_event__date_userform %}
{% assign color_for_big_event = options.color_for_big_event__color_required_userform %}

{% # This will NOT appear on the run task user form %}
{% assign level = options.level__select_o1_low_o2_high %}

{% if event.topic == "mechanic/user/form" %}
  {% # we need to get the value from user input, we don't want the value from the task option %}
  {% assign big_event = input.the_big_event %}
  {% assign color_for_big_event = input.color_for_big_event %}

{% endif %}

{% action "echo" big_event, level, color_for_big_event %}
```

{% hint style="info" %}
Note: `event.data` contains submitted values for backward compatibility. Prefer `input.<name>` going forward, and use `options.*` for stored task options.
{% endhint %}

User form options also appear on the Run tasks page for Shopify admin action links (mechanic/user/{resource} topics). In that context, `input.<name>` is still the recommended way to access user input.
