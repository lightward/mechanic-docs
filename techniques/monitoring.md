# Monitoring Mechanic

Mechanic is frequently used for business-critical operations. The Mechanic team closely monitors its operations (and shares status information at [status.mechanic.dev](https://status.mechanic.dev)), but it can be useful to set up task-specific monitoring, specific to the visibility needs of each business.

{% hint style="info" %}
Mechanic is a highly available system with built-in redundancies. It processes data asynchronously, using queues — this means that, generally, any failures simply result in a delay, rather than resulting in lost data.

If you're experiencing delayed events, start here: [Why are my tasks delayed or not running?](https://learn.mechanic.dev/faq/why-are-my-tasks-delayed-or-not-running)
{% endhint %}

## Monitoring the Mechanic platform

Mechanic's platform status is available at [status.mechanic.dev](https://status.mechanic.dev). The system metrics area contains graphs providing realtime indication of how quickly Mechanic is processing runs, as well as information about observed delays in events coming from Shopify. (These can be useful in diagnosing whether an emergent issue is caused by Mechanic, or if it's a problem on Shopify's end.)

Mechanic's status page also supports alert subscriptions, via email, SMS, Slack, and webhooks. If Mechanic is an important part of your operations, we strongly recommend subscribing to status alerts.

## Monitoring tasks

Mechanic does not have native alerting for task or action runs that return errors. To achieve task-specific monitoring, use the [HTTP action](../core/actions/http.md) to ping a service like [Cronitor](https://cronitor.io) when critical tasks run, configuring the service to send alerts should the pings ever miss their schedule.

It's generally preferable to use an external service for this sort of thing, rather than using Mechanic to monitor itself. Still, monitoring tasks _are_ viable, by using [a scheduler event](https://learn.mechanic.dev/platform/events/topics#scheduler) to check on an expiring flag in the [Mechanic cache](https://learn.mechanic.dev/platform/cache). This way, by setting that flag during sensitive task runs (using the[ Cache action](https://learn.mechanic.dev/core/actions/cache)), a sort of [dead man's switch](https://en.wikipedia.org/wiki/Dead\_man's\_switch) can be created: if the scheduled run ever finds that the flag is not present, that task could then send an email (or post to Slack, or whatever's useful).

### Example

When run manually, this task sets an auto-expiring flag in the Mechanic cache, set to expire in ten minutes. Separately, this task checks for the presence of that flag every ten minutes. If the flag is missing (indicating that the task was not run manually in the last ten minutes), the task sends an email.

{% tabs %}
{% tab title="Subscriptions" %}
```
mechanic/scheduler/10min
mechanic/user/trigger
```
{% endtab %}

{% tab title="Code" %}
```liquid
{% raw %}
{% if event.topic contains "mechanic/scheduler" %}
  {% if cache["monitor-10min"] %}
    {% log ok: true, cache: cache["monitor-10min"] %}
  {% else %}
    {% action "email" %}
      {
        "to": "alerts@example.com",
        "subject": "Monitor triggered!",
        "body": "Problem!"
      }
    {% endaction %}
  {% endif %}
{% elsif event.topic == "mechanic/user/trigger" %}
  {% assign now = "now" | date: "%Y-%m-%d %H:%M %p" %}
  {% action "cache", "setex", "monitor-10min", 600, now %}
{% endif %}
{% endraw %}
```
{% endtab %}
{% endtabs %}
