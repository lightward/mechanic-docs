# Can Mechanic help before an order is placed?

Yes. A [task](../core/tasks/) runs in response to an event. For example, a customer can submit a [quote request from their cart](../app/forms.md#example-request-a-quote-from-the-cart), and a task can create a draft order for your team to review. A task subscribed to `shopify/orders/create` runs after Shopify creates the order. For an online purchase, that’s too late to change what the buyer saw at checkout.

If you need to change what the buyer sees **during checkout**, [Functions](../core/functions.md#what-can-a-function-do) are available in a limited pilot, enabled shop by shop. Shopify can apply a discount, check whether the cart meets a requirement, expand or merge bundle items, or change which eligible delivery and payment options the buyer sees.

For a gift order, a storefront form can collect the buyer’s message. A validation Function can block checkout if a wrapped item is missing its message. After fulfillment, a task can email the message to the recipient. [Walk through the gift-wrap example](../resources/tutorials/require-a-gift-message-for-wrapped-items.md).
