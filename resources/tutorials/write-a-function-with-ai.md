# Write a Function with your AI assistant

Suppose you want to offer three items from a collection for $30. You can describe the offer to your AI assistant, then review the rule and its test carts in Mechanic. This walkthrough uses a sample cart in USD.

1. Install the [Mechanic Function writer skill](../../platform/agent-skills.md) in your AI tool.
2. Ask for the rule and its examples together:

   > Create a Mechanic Function that charges up to $30 for each complete group of three one-time items from a collection I choose. Choose the cheapest eligible items first. Leave any extra items at their selling price. Include saved carts with two, three and four qualifying items. Give me a portable Function JSON file.

3. In Mechanic, open **Functions → Import function** and choose the file. Select your shop’s collection for **Eligible collection**.
4. In **Test cart**, check each saved example. With these sample prices, you should see:

   | Items | Before discount | Savings | Buyer pays |
   | --- | ---: | ---: | ---: |
   | $18 + $14 | $32 | $0 | $32 |
   | $18 + $18 + $14 | $50 | $20 | $30 |
   | $18 + $18 + $18 + $14 | $68 | $20 | $48 |

5. Save the draft. Choose **Try with my products** to check your own prices and quantities. When you’re ready to change checkout, review any requested Shopify permissions and publish the saved version.

## Edit an existing Function

Open the rule and choose **View definition**. Give the copied JSON to your assistant and describe the change. Paste the edited JSON into the same panel and choose **Review changes**. Compare the existing and proposed results for each saved cart. **Apply to draft** takes the change back to the visual editor. Existing named carts and their expectations are kept. Use a new name when adding a case.

If you keep your rules in Git, the [Mechanic CLI](../../platform/mechanic-cli.md#function-drafts) can preview the same carts and save a draft to your shop.
