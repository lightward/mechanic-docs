# Flow

The **Flow** action sends data to Shopify Flow, arriving as one of four possible Flow triggers.

{% hint style="info" %}
This page is about the Mechanic action that sends data to Shopify Flow. For a review of Mechanic's entire integration with Flow, see [Shopify Flow](../../../platform/integrations/shopify-flow.md).
{% endhint %}

## Options

### Resource options

The Flow action accepts at most one resource option, identifying a specific Shopify resource, and resulting in a resource-specific Flow trigger. If no resource option is provided, Mechanic will use the General trigger.

{% hint style="warning" %}
These resource options only accept fully-numeric resource IDs (i.e. 12345). They do not accept global IDs (i.e. gid://shopify/Customer/12345).
{% endhint %}

| Resource option                    | Flow trigger                  |
| ---------------------------------- | ----------------------------- |
| `customer_id`                      | "Mechanic sent customer data" |
| `product_id`                       | "Mechanic sent product data"  |
| `order_id`                         | "Mechanic sent order data"    |
| (when no resource option is given) | "Mechanic sent general data"  |

### Data options

This action also sends user-defined data, with one option available for each of Flow's supported datatypes. These options are _always_ sent to Flow, even if they're omitted from the action definition; when omitted, their values are set to the documented default.

| Option         | Type          | Default                       |
| -------------- | ------------- | ----------------------------- |
| `user_boolean` | Boolean       | `false`                       |
| `user_email`   | Email address | `"hey@mechanic.invalid"`      |
| `user_number`  | Number        | `0`                           |
| `user_string`  | String        | `""`                          |
| `user_url`     | URL           | `"https://mechanic.invalid/"` |

## Usage

For a detailed review of usage, see [Shopify Flow](../../../platform/integrations/shopify-flow.md#mechanic-flow).

