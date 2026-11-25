---
description: Create a discount with your AI assistant, test its example carts, and review it in Mechanic.
---

# Write a Function with your AI assistant

Suppose you want to offer three items from a collection for $30. You can describe the offer to your AI assistant, then review the rule and its test carts in Mechanic. You’ll need access to the [Functions pilot](../../core/functions.md) to import the rule. This walkthrough uses sample items priced in USD.

In a shop with Function authoring enabled in Ask Mechanic, you can ask directly in the app. Describe the rule and the carts you want to test. After Ask Mechanic validates the definition, choose **Open in builder** to review it as a new unsaved Function. Choose **Save draft** to keep it. Publishing is a separate step.

### Use an external AI assistant

1. Install the [Mechanic Function writer skill](../../platform/agent-skills.md) in your AI tool. If you don’t have the skill yet, open a discount in the builder, choose **View definition**, and give your assistant the **JSON schema** and **Discount example** from that panel.
2. Ask for the rule and its examples together:

   > Create a Mechanic Function that charges up to $30 for each complete group of three one-time items from a collection I choose. Choose the cheapest eligible items first. Leave any extra items at their selling price. Include saved carts with two, three and four qualifying items. Use a regular item at $18 and a preorder item at $14. Show one of each for two, two regular and one preorder for three, and three regular and one preorder for four. Give me a portable Function JSON file with each cart’s expected savings.

3. In Mechanic, open **Functions → Import function** and choose the file or paste the JSON, then choose **Import as draft**.
4. In **Test cart**, use **Choose a test cart** to switch between the saved examples. With these sample prices, you should see:

   | Items | Before discount | Savings | Buyer pays |
   | --- | ---: | ---: | ---: |
   | $18 + $14 | $32 | $0 | $32 |
   | $18 + $18 + $14 | $50 | $20 | $30 |
   | $18 + $18 + $18 + $14 | $68 | $20 | $48 |

5. Under **Rule logic**, edit **Discounted items** and select the collection that qualifies in your shop. Choose **Save draft**, then **Try with my products** to check your own prices and quantities. When you’re ready to change checkout, review any requested Shopify permissions and publish the saved version.

## Edit an existing Function

Open the rule and choose **View definition → Copy definition**. Give the JSON to your assistant and describe the change. Paste the edited JSON into the same panel and choose **Review changes**. Compare the existing and proposed results for each saved cart. **Apply to draft** takes the change back to the visual editor. Existing named carts and their expectations are kept. Use a new name when adding a case.

If you keep your rules in Git, the [Mechanic CLI](../../platform/mechanic-cli.md#function-drafts) can preview the same carts and save a draft to your shop.
