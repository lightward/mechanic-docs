---
description: Connect your Mechanic tasks to customer requests and information on your storefront, after checkout, and in customer accounts.
---

# Extensions

Bring your Mechanic tasks to the places customers use. Collect a quote request on your storefront, let a buyer ask for help with an order, or show a member their benefits when they sign in.

**Forms collect requests. Cards show information. A customer page brings them together. Tasks handle the work.**

Open **Extensions** in Mechanic to create and manage these experiences. Choose a destination to browse its templates, or open something you have already created from the shared list.

{% hint style="info" %}
If your menu says **Forms**, Customer accounts is not enabled for your shop yet. Use **Forms** for your available destinations; **Create form** and **Import form** become **Create extension** and **Import extension** when Customer accounts is enabled. Your existing forms and connected tasks stay in place.
{% endhint %}

## Choose a destination

| Destination | What you can build | Example |
| --- | --- | --- |
| [Storefront](forms.md) | Forms placed in your Shopify theme | Collect a quote request, or save a gift message with the cart |
| [Thank you & order status](thank-you-and-order-status-forms.md) | Forms shown after checkout or on an order | Ask a post-purchase survey or collect delivery instructions |
| [Customer accounts](customer-accounts.md) | Request forms, information cards, and a dedicated customer page | Collect a trade application, share a response, or show membership information |

The destination determines the available fields and placements. Your theme supplies the appearance of storefront forms. Shopify supplies native controls for Thank you, Order status, and customer accounts.

## Start with a template, then connect tasks

1. Choose a destination and a template. The template creates an editable draft.
2. Customize its questions or content. For a form that sends requests, choose its webhook in **Submission settings**.
3. Save and publish the form or card so it appears in the matching task's picker.
4. Install, configure, and enable the tasks you need. Follow their setup instructions and approve any required access.
5. Place it in your theme or Shopify's checkout and accounts editor, then test the complete experience.

Templates do not install tasks. A customer page reuses the forms, cards, and tasks you connect; the page does not need its own task. [Cart-saving storefront forms](forms.md#save-answers-to-the-cart) save directly to Shopify instead of sending a webhook, and tasks can use their answers after checkout.

## What happens after a customer submits?

For forms connected to a webhook, the submission creates an ordinary Mechanic event. Subscribed tasks run in the background and perform actions such as saving answers, emailing your team, or updating Shopify. A confirmation acknowledges the submission; it does not mean those actions have finished.

For customer accounts and tracked order requests, tasks can save a customer-facing status and message. The customer refreshes to read that saved result. Opening or refreshing the extension does not run a task.

Keep your existing names for the work inside Mechanic: [events](../core/events/README.md) trigger [tasks](../core/tasks/README.md), and tasks produce [actions](../core/actions/README.md). Extensions give customers a way to interact with that work.
