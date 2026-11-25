# Can I re-send order confirmation emails with Mechanic?

As of this writing, Shopify doesn't have a simple API for re-sending order confirmation emails. \(If this is important to you, send an email to [support@shopify.com](mailto:support@shopify.com) and let the folks there know that this is affecting your business.\)

However, you can use Mechanic to trigger order-related emails on demand ([sample task](https://usemechanic.com/task/trigger-order-emails-with-a-tag)).

To match the appearance of your store's existing emails, [forward or import one into the email template editor](../platform/email/templates.md#start-with-an-email-you-already-send). The task supplies the new message and order details; importing a sent email does not recover its Liquid logic.

To adapt the full HTML/Liquid template, including its order-detail logic, see [Migrating templates from Shopify to Mechanic](../techniques/migrating-templates-from-shopify-to-mechanic.md).
