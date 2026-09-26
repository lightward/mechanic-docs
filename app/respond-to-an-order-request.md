---
description: Connect a delivery-instructions form to tasks, review a customer's request, and share a response on their order.
---

# Respond to an order request

A customer wants a parcel left at their building's reception desk. They send a delivery-instructions request from their order. Your team reviews it, handles the request, and leaves a response the customer can check later.

This walkthrough uses the **Delivery instructions request** template and the save and update tasks. You can add an email task to notify your team. The form collects the request; your team or a separate task still needs to arrange any change with the people fulfilling the order.

## Set up the form

1. In Mechanic, open **Forms → Create form → Thank you & order status** and choose **Delivery instructions request**.
2. Keep **Order status page** selected. You can also keep **Thank you page** selected to collect the request immediately after checkout. Replies are read on Order status after the customer signs in.
3. Review the questions. The starter asks **What would you like us to know?** and is available while the order is unfulfilled.
4. In **Submission settings**, choose a [Mechanic webhook](../platform/webhooks.md) for these requests. Keep **Show saved request progress** on so the customer can return to check your response.
5. Save and publish. This makes the form available in the tasks' **Form** picker. Connect the tasks below before placing the form on customer pages.

## Connect the tasks

Install [Save a Mechanic Order status request](https://tasks.mechanic.dev/save-a-mechanic-order-status-request):

1. Choose **Delivery instructions request** in its **Form** option.
2. Set **Webhook event topic** to the exact topic shown in the form's Submission settings.
3. Use **Received** for the initial status and a message such as **We've received your request and will review what we can arrange.**
4. Save the task, approve its required permissions, and run its one-time storage setup as described in the task's instructions. Enable the task.

Install [Update a Mechanic Order status request](https://tasks.mechanic.dev/update-a-mechanic-order-status-request). Choose the same form, save and enable it. Keep **Send customer email** off for the first test: the response will still be available on the customer's Order status page.

To notify staff when a request arrives, also connect [Email a Mechanic Order status request](https://tasks.mechanic.dev/email-a-mechanic-order-status-request). Choose the same form and webhook topic, then set your team's recipients. The email task is optional and runs independently of saving the request.

The save task receives the customer's submission. The update task runs when staff respond from an order. The form template does not install either task for you.

## Let a customer send a request

In Shopify's checkout and accounts editor, add the **mechanic-order-status** app block to **Order status** and save. If you selected Thank you for the form, add the block separately to that page too. One block lists the eligible published forms for that page.

Use an unfulfilled test order belonging to a customer account you can sign into. Open its Order status page, sign in as that customer, and open **Delivery instructions request**. Enter an example request:

> Please leave my parcel with reception. They are open until 6 pm.

Choose **Send request**. The confirmation means the request was sent; the receiving task still needs to finish saving it.

In Mechanic, open the form's **Submission settings → View events for this webhook topic**. Open the new event, review the submitted answers, and confirm the save task and its Shopify action succeeded. The request is now saved against that order. If you connected the staff email task, check its result too.

## Review and respond from the order

Review the request and arrange any delivery change before promising it to the customer. Updating the request's status does not change the order or send instructions to a carrier.

1. Open the same order in Shopify.
2. Choose **More actions → Send to Mechanic** and select your configured **Update a Mechanic Order status request** task. See [Shopify admin action links](../core/shopify/admin-action-links.md).
3. In the run form, enter a **Status** and **Customer message**. After handling this example request, you could use **Resolved** and **We've passed your reception instructions to our fulfillment team.**
4. Run the task and confirm its Shopify action succeeded.

Both fields are customer-facing. Keep private staff notes elsewhere. The update task preserves the original answers and updates the latest saved request for this form and order.

## Check the customer's response

Return to Order status as the customer who owns the order. Open the form and choose **Check status** to see the saved status and message. The customer should now see **Resolved** and your response.

The delivery-instructions starter allows another request after the previous one is **Resolved** or **Declined**. This only applies while the order still meets the form's availability rules. A different signed-in customer must not be able to read the response.

There is no separate reply inbox or conversation history: staff use the order action and the customer sees the latest saved status and response. For a survey that only needs to save answers, [send it to Google Sheets](thank-you-and-order-status-forms.md#send-a-survey-to-google-sheets) and leave saved request progress off.
