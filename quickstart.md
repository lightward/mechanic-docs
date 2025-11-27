# 🚀 Quickstart: Build your first Mechanic task

This is the fastest path to go from install to a working task. It uses a manual trigger so you can test without waiting for a Shopify webhook, and it sends a simple email so you can see an action run end to end.

## 1) Start a new task

- Install Mechanic from the Shopify App Store (or open it if you already have it).
- Create a new task, give it a name, and add this subscription so you can trigger it on demand:

```text
mechanic/user/trigger
```

## 2) Add an option (recipient)

- Add a task option named `recipient__email_required`. Mechanic will render this as an email field in the task form.

## 3) Paste this code

```liquid
{% if event.topic == "mechanic/user/trigger" %}
  {% action "email" %}
    {
      "to": {{ options.recipient__email_required | json }},
      "subject": "Hello from Mechanic",
      "body": {{ "This task ran on " | append: event.topic | json }},
      "from_display_name": {{ shop.name | json }}
    }
  {% endaction %}
{% endif %}
```

What this does:
- On a manual trigger, it sends an email to the configured recipient.
- It uses the action tag to define the Email action.

## 4) Preview and run

1. Click **Preview** and run it. You should see the rendered email JSON and no errors.
2. Click **Run task** and submit the form with a real email address. You’ll get the email when the action runs.

## 5) Go live and adapt

- Toggle the task on when you’re happy.
- Swap the subscription for a Shopify event (e.g. `shopify/orders/create`) and adjust the code to use data from `event`.
- Add more actions (HTTP, Shopify GraphQL, Files) and options as needed.

## Next steps

- Learn more about [tasks](core/tasks/README.md), [events](core/events/README.md), and [actions](core/actions/README.md).
- See [Defining preview events](core/tasks/previews/events.md) to give previews real data.
- Browse real-world patterns in the [task library](resources/task-library/README.md).
