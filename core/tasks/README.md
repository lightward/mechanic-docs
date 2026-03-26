---
description: "Tasks are the building blocks of Shopify automation in Mechanic — Liquid code that responds to events and performs actions like tagging, emailing, and syncing data."
---

# Tasks

A task is an automation unit — it watches for something to happen in your store and responds with one or more actions. A task might tag every order over $100, send a notification when inventory is low, or sync customer data to a spreadsheet.

Under the hood, a **task** combines Liquid code with user-configurable [**options**](options/) and a set of event [**subscriptions**](subscriptions.md). When an event matches a subscription, Mechanic executes the task's code with access to the event data. The code can query the Shopify Admin API, apply conditional logic, iterate over data, and render any number of [**actions**](../actions/) — Shopify mutations, emails, HTTP requests, file generation, and more.

A task uses its [**preview**](previews/) to communicate ahead of time the work it intends to do. Previews are important for users, and are also important for Mechanic itself – Mechanic looks to the task preview to understand what [**permissions**](permissions.md) a task requires. Tasks can also declare their permissions explicitly using the [`{% permissions %}` tag](../../platform/liquid/tags/permissions.md).

Tasks may be written from scratch, or installed from the Mechanic library (available in-app and [on GitHub](https://github.com/lightward/mechanic-tasks)). Once installed, a task's code may be modified at any time.

{% hint style="info" %}
Working on getting better at task-writing? See [Practicing writing tasks](../../resources/tutorials/practicing-writing-tasks.md), and [Writing a high-quality task](../../techniques/writing-a-high-quality-task.md).
{% endhint %}

## Example

This very basic task subscribes to shopify/customers/create, and renders an [Email action](../actions/email.md), using an email subject and body taken from user-configured [options](options/).

**Subscriptions**

```
shopify/customers/create
```

**Code**

```liquid
{% action "email" %}
  {
    "to": {{ options.email_recipient__email_required | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body__multiline_required | newline_to_br | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
```

**Export**

```
{"name":"Customer signup alerts","options":{"email_recipient__email_required":"aesha@example.com","email_subject__required":"A new customer has signed up: {{ customer.email }}","email_body__multiline_required":"Hi! View this customer's details online:\n\nhttps://{{ shop.domain }}/admin/customers/{{ customer.id }}\n\n-Mechanic"},"script":"{% action \"email\" %}\n  {\n    \"to\": {{ options.email_recipient__email_required | json }},\n    \"subject\": {{ options.email_subject__required | json }},\n    \"body\": {{ options.email_body__multiline_required | newline_to_br | json }},\n    \"from_display_name\": {{ shop.name | json }}\n  }\n{% endaction %}","subscriptions":["shopify/customers/create"],"online_store_javascript":null,"order_status_javascript":null,"docs":null,"subscriptions_template":"shopify/customers/create","shopify_api_version":"2022-04","liquid_profiling":false,"perform_action_runs_in_sequence":false,"halt_action_run_sequence_on_error":false,"preview_event_definitions":[]}
```

![](<../../.gitbook/assets/Screen Shot 2022-04-01 at 7.14.46 PM.png>)
