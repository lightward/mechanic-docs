---
description: Build, test, and publish Shopify rules for discounts, bundles, and checkout in Mechanic.
---

# Functions

A function is a rule that Shopify applies while someone shops. Use a function to give a buyer a discount while their order is still in the cart. Use a [task](tasks/README.md) to act on an event, such as emailing your team after the order is placed. You can build a function on its own, without writing code or installing a task.

{% hint style="info" %}
Functions are in development for a limited pilot. The Mechanic team enables new function creation for individual pilot shops.
{% endhint %}

## What can a function do?

| Function type | Example |
| --- | --- |
| Discount | Give a customer 15% off a mug when they buy three coffees. You can discount items, an order, or shipping. |
| Cart and checkout validation | Ask a buyer to place separate orders for preorder and regular items, and show them what to change. |
| Cart Transform | Turn a tasting kit into its component items, or combine eligible items into a bundle. |
| Delivery customization | Hide express shipping for items that need extra preparation time. |
| Payment customization | Rename a manual payment method so buyers recognize it, or change where it appears among eligible methods. |

## Try a rule with a test cart

1. In Mechanic, open **Functions** and choose **Create function**. Pick a type to build from scratch, or choose a starting point. **View all starting points** opens the library.
2. Open **Buy three coffees, save on a mug**. The rule counts one-time coffee purchases. When there are at least three, it takes 15% off one ceramic mug already in the cart.
3. In **Test cart**, switch between the three example carts. Three coffees and a mug earn the discount. Two coffees and a mug earn nothing. Three coffees without a mug meet the condition, but there is no mug to discount. Open **Why this result?** to see which path ran.
4. To try a more complex offer, choose **Add an otherwise outcome** and give 5% off the order. Test the same carts again. Two coffees and a mug now get 5% off. Three coffees without a mug still get no discount because the first path matched. To give that cart the fallback too, add a condition to the first path requiring a mug.

To test your own merchandise, choose the products or collections used by the rule, then select **Try with my products** in the test cart. With real, one-time variants in the cart, **Open in storefront** opens those items and quantities in a new tab. Shopify uses its current prices and published rules. Simulated customer tags, addresses and other test inputs stay in the builder.

## Save, publish, and turn off a rule

**Save draft** keeps your work in Mechanic. You can save a test cart with its expected result and use it again after an edit. If an earlier version of the rule is live, shoppers continue to get that version.

Before publishing a starter, replace any sample products and collections with your shop’s selections and save the draft. If any are missing during review, choose **Choose missing resources**.

When you’re ready, open **Live checkout**. Review any Shopify permissions the rule needs, then choose **Publish saved version** and **Confirm publish**. Wait for Shopify’s confirmation. If the change is still awaiting confirmation, use **Check publication again**. For a scheduled discount, its start and end dates determine when shoppers can receive it.

If checkout gives a different result than your draft, choose **Test published version** under **Live checkout**. It runs the same test cart against the last confirmed rule. While Shopify is confirming a change, this button reads **Test last confirmed version**. To stop using a live rule, choose **Deactivate** and confirm. For a saved draft that has never had a publication attempt, open **Function actions** and choose **Delete draft**.

To reuse a rule in another shop, choose **Function actions → Export JSON**, then **Import function** in the destination shop. Select that shop’s products and collections, test the imported draft, and publish it when ready.
