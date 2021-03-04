# Email Templates

Sending email with Mechanic, using [the "email" action](../core-concepts/actions/action-types/email.md)? Use templates to make sure your emails match your brand.

_Don't forget, you can also_ [_send email from your store's domain_](https://docs.usemechanic.com/article/368-sending-mail-from-your-stores-domain)_!_

## Instructions

### 1. Navigate to your Mechanic account settings.

From the main Mechanic screen, scroll down to your account summary to find this link.![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e704286364bc9443fb/5e28a1e7622fb.png)

### 2. Find the "Email templates" setting, and click the "Add template" link.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e72c7d3a7e9ae69be2/5e28a1e7a1421.png)

### 3. Replace the default template with the HTML/CSS of your choice.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e804286364bc9443fc/5e28a1e7eaaba.png)

Use the `{{ body }}` and `{{ subject }}` variables to insert the actual content of your email. Use the more specific guides below to adapt existing email templates for Mechanic.

## Using multiple templates

Mechanic will always default to using the template named "default", when present. Feel free to add additional templates – just remember to update your task\(s\) to use the appropriate template.

Here's an example email action, specifying a non-default template:

```cpp
{
  "action": {
    "type": "email",
    "options": {
      "to": "hello@example.com",
      "subject": "Hello world",
      "body": "It's a mighty fine day!",
      "template": "welcome"
    }
  }
}
```

## Adapting existing templates

### Shopify

If you'd like your emails to match your existing Shopify messages, use these steps:

1. Navigate to Settings &gt; Notifications, in your Shopify admin.

2. In the list of "Customer notifications" templates, scroll down to the "Customer" section, and open the "Contact customer" template. \(If you want to set up Mechanic emails that resemble a different notification template, choose that template instead.\)  
  
3. Copy all the code in the "Email body \(HTML\)" field.  
  
4. Head over to Mechanic's email template settings \(see instructions above\), and paste in your copied code as the Mechanic email template. Mechanic will automatically adapt any Shopify-specific display code for you, with the exception of the accent color and email logo.  
  
Important: To have this new template apply to all Mechanic emails, leave the template labeled "default". Otherwise, choose a new name as appropriate.

5. If you're bringing over an order confirmation template, or something else that uses really dynamic data, you may need to migrate Shopify's notification variables to Mechanic's Liquid objects. [Learn more about this](https://docs.usemechanic.com/article/343-migrating-templates-from-shopify-to-mechanic)  
  
6. Optional: To bring over your email logo, go back to the "Notifications" area in Shopify, and click on the "Customize" button, under "Customer notifications". You'll see a setting for "Accent color". Click on the block of color, then copy the string of text that looks like "\#ABCDEF". This is your color accent code. :\) Back in your Mechanic email template, replace the two instances of "\#1990c6" \(the default accent color\) with the color code that you copied, then save your settings.

7. Optional: To bring over your shop's logo, see our [How do I send images with my emails?](https://docs.usemechanic.com/article/342-how-do-i-send-images-with-my-emails) guide – see the "Embedding images" section.

### OrderlyEmails

If you're using OrderlyEmails, find the code for the "Blank" template, copy its code, and paste it in as your Mechanic email template.

On the display and formatting side of things, no further changes to the code are necessary – Mechanic will take care of making sure that the right variables are inserted. :\)

If you're bringing over an order confirmation template, or something else that uses really dynamic data, you may need to migrate Shopify's notification variables to Mechanic's Liquid objects. [Learn more about this](https://docs.usemechanic.com/article/343-migrating-templates-from-shopify-to-mechanic)

