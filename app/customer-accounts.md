---
description: Collect customer requests, show personal information, and bring forms and cards together on a customer account page powered by Mechanic tasks.
---

# Customer accounts

Give customers a place to ask for help, apply for a trade account, or check their membership information. Your Mechanic tasks handle the request or save the information each customer sees.

Open **Extensions → Customer accounts** to choose a template. This destination uses Shopify's new customer accounts and requires the customer to sign in. If your menu still says **Forms**, Customer accounts is not enabled for your shop yet. See [Extensions](extensions.md) for the destinations available in Mechanic.

## Choose what to build

| Start with | What the customer sees | Tasks to connect |
| --- | --- | --- |
| Membership information | Their membership status and benefits | [Publish customer membership information](https://tasks.mechanic.dev/publish-customer-membership-information) reads your membership tag and saves the display text |
| Trade account application | An application and its latest response | [Save a Mechanic customer account request](https://tasks.mechanic.dev/save-a-mechanic-customer-account-request) and [Update a Mechanic customer account request](https://tasks.mechanic.dev/update-a-mechanic-customer-account-request) |
| Service and warranty request | A request for help and its latest response | The same save and update tasks, configured for this form |
| Account information | A personal message, useful details, and an optional link | [Publish customer account information](https://tasks.mechanic.dev/publish-customer-account-information) lets staff supply the content; customize it for automatic updates |
| Your customer page | Your chosen forms and cards in one place | Uses the tasks already connected to its forms and cards |

A **form** collects a request. A form with no questions works as a request button. A **card** shows information a task has saved for that customer. A **customer page** brings your forms and cards together.

Templates create drafts, not installed automation. You can edit every question and message, and use your own tasks. An application marked **Approved** does not itself grant trade pricing or membership; those changes belong in your tasks or staff process.

## Set up a request and response

1. Choose **Service and warranty request** or **Trade account application** and edit the questions. Use **Try form** to check the questions without sending a request.
2. In **Submission settings**, choose a [Mechanic webhook](../platform/webhooks.md). Save and publish the form so it appears in task pickers.
3. Install **Save a Mechanic customer account request**. Select this form in its **Form** option and use the webhook's exact topic for **Webhook event topic**.
4. Save and enable the task, approve its requested permissions, and run it once from Mechanic as described in its instructions. This creates the private storage definition for this form's requests.
5. Install **Update a Mechanic customer account request**, choose the same form, and enable it. Place the form as described below.

After a customer submits, the save task records their latest request. Staff can review its answers in the Mechanic event or the customer record's saved request data. To respond, open the customer in Shopify, choose **More actions → Send to Mechanic**, and run your configured update task. Choose a status and enter the message the customer should see. See [Shopify admin action links](../core/shopify/admin-action-links.md).

The customer sees the updated status and message after the task's Shopify action succeeds and they refresh. Keep staff-only notes out of these fields. An order number entered in a form is a reference to check, not verified order ownership; use [Order status forms](thank-you-and-order-status-forms.md) for requests tied to a verified order.

### Optional emails

The save task can notify your team after a successful save. The update task can email the customer after a successful response update. Both are off by default; select the notification option in the corresponding task and complete Mechanic's normal [email approval](../platform/email/README.md).

You can choose an optional saved [email template](../platform/email/templates.md) in that task. A blank selection uses the shop's default layout. Choosing a layout does not enable notifications. Team emails link to the customer in Shopify; customer emails link back to their account.

## Show an information card

1. Choose **Account information** and edit its heading and introduction. Save and publish the card.
2. Install **Publish customer account information** and select the card in its **Card** option. Save and enable the task, approve its access, then run it once from Mechanic to create its private storage definition.
3. Open a customer in Shopify, choose **More actions → Send to Mechanic**, and run this task. Enter a status, optional message, and up to eight details as label/value pairs. Add a link label and complete HTTPS URL if needed.
4. After the task succeeds, sign in as that customer and refresh the card's page. Other customers do not see this customer's information.

Cards stay hidden until a task saves information for that customer. To hide a card again, run the publishing task with **Hide card** selected. To restore it, clear that option and supply the new content. Use one publishing task per card to avoid competing updates.

The generic publishing task is a starting point: customize its subscriptions and Liquid to compute updates from your store's data or integrations. For tag-based membership information, use **Membership information** and its matching task instead. That task reads your existing membership tag; it does not sell, renew, or grant membership. Changes apply when the task runs for a customer, not as an automatic backfill of all customers.

Card text is plain text. Links can lead to instructions, benefits, or another service, but do not grant access to their destination. Keep secrets and private staff notes out of customer-facing content.

## Choose where customers find it

### Profile and Orders blocks

In the form or card's **Where customers find this** settings, select **Profile page**, **Orders page**, or both. Save and publish those choices.

In Shopify's checkout and accounts editor, add the **mechanic-customer-account** block to each selected page and save. One block shows the eligible published forms and cards for that placement. Selecting a placement in Mechanic does not add the block to Shopify automatically.

### A dedicated customer page

1. Create **Your customer page** from the templates. Give it a heading customers will recognize, such as **Your account services**.
2. Under **Page sections**, add your existing Customer accounts forms and cards. Use **Up** and **Down** to choose their order. If the list is empty, save the page and create a form or card first, then return to add it.
3. Publish each form or card and the page. The page reuses their connected tasks and visibility rules; it needs no additional task.
4. In Shopify's checkout and accounts editor, add the **mechanic-customer-page** page and its link to the customer account menu. Give the menu item a name customers will recognize. [Shopify supports adding these pages to account navigation](https://shopify.dev/docs/apps/build/customer-accounts/full-page-extensions#allow-or-prevent-direct-linking).

Mechanic supports one dedicated customer page per shop, with up to 20 sections. A form or card can appear on this page and in an account block using the same published content. Reordering sections changes only the page. Publishing in Mechanic does not edit your Shopify menu.

## Visibility and repeat requests

Customer-tag conditions control who can start a request or see a card. Mechanic checks the signed-in customer; entering someone else's email or customer ID does not change that identity.

Request forms can allow repeat requests anytime, once per customer, or after the previous request is resolved. With the last option, **Resolved** and **Declined** allow another request; **Approved** does not reopen it. The form must still allow new requests for that customer.

Customers can read the response to their latest request even if they stop matching the conditions for new requests. Unpublishing removes the form and its response from customer accounts. Hiding an information card only hides that card's content.

## Test the complete experience

Use test customer accounts and information you can safely share. Check the form or card in its actual Shopify placement on desktop and mobile. Builder and editor previews do not submit requests or show real customer information.

For a request, submit it, inspect the Mechanic event and successful task actions, then respond from the customer in Shopify. Return as that customer and refresh to see the response. For a card, save information, refresh to see it, then hide and restore it. An empty customer page still offers **Refresh information**.

Check a second signed-in account too: it must not see the first customer's personal information. If you enable email, use an inbox you control and verify the message arrived after the save succeeded.

## What is supported?

Customer accounts supports the shared questions, conditional fields, steps, and one-question-at-a-time layouts. Shopify supplies the native controls and appearance. File uploads, cart capture, order-item selection, and theme styling are not supported here.

Request tracking keeps the latest request per customer and form, not a conversation history. Reading a page or refreshing saved information never runs a task. Tasks own the business rules and changes to Shopify.

For custom tasks, see the [Customer accounts event object](../platform/liquid/objects/event.md#customer-account-request) and [form and card option flags](../core/tasks/options/README.md). Required access uses Mechanic's normal [permission calculation](../core/tasks/permissions.md#permissions-for-other-mechanic-features).
