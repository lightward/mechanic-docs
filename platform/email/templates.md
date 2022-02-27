# Email templates

A Mechanic account may be configured with one or more **email templates**, used by the [**Email action**](../../core/actions/email.md) to render email content using a Liquid that can be shared across tasks. Email templates have access to Liquid [**template variables**](../../core/actions/email.md#creating-template-variables) named after each option in the Email action, including any custom options added by the task author.

Unless configured otherwise, each email will use the email template named "default", if it exists.

{% hint style="info" %}
To learn more about formatting messages with HTML, CSS, and images, see [Message formatting](../../core/actions/email.md#message-formatting).
{% endhint %}

## Configuration

Navigate to your Mechanic settings (it's the "Settings" link in the upper right), and head to the "Email templates" tab. Click the "New email template" link.

![](<../../.gitbook/assets/Screen Shot 2022-02-27 at 10.53.14 AM.png>)

Replace the default template with the HTML and CSS of your choice. Use [email template variables](../../core/actions/email.md#creating-template-variables) to pull in values from the Email action. Learn about adapting existing templates below.

## Using multiple templates

Mechanic will always default to using the template named "default", when present. Feel free to add additional templates – just remember to update your task(s) to use the appropriate template.

Here's an example email action, specifying a non-default template:

```cpp
{% raw %}
{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": "It's a mighty fine day!",
    "template": "welcome"
  }
{% endaction %}
{% endraw %}
```

## Adapting existing templates

{% hint style="warning" %}
Templates from other sources, like Shopify and OrderlyEmails, use different Liquid variables than Mechanic does. It's very important to check variable usage to make sure any Liquid variable usage is compatible with Mechanic.

Learn more: [Interacting with Shopify / REST via Liquid objects](https://learn.mechanic.dev/core-concepts/interacting-with-shopify#rest-via-liquid-objects).
{% endhint %}

### Shopify

If you'd like your emails to match your existing Shopify messages, use these steps:

1. Navigate to Settings > Notifications, in your Shopify admin.
2. In the list of "Customer notifications" templates, scroll down to the "Customer" section, and open the "Contact customer" template. (If you want to set up Mechanic emails that resemble a different notification template, choose that template instead.)
3. Copy all the code in the "Email body (HTML)" field.
4. Head over to Mechanic's email template settings, and paste in your copied code as the Mechanic email template, using the "default" template to have this apply to all emails, or using a custom email template name. Mechanic will automatically adapt any Shopify-specific display code for you, with the exception of the accent color and email logo.
5. If you're bringing over an order confirmation template, or something else that uses really dynamic data, you may need to migrate Shopify's notification variables to Mechanic's Liquid objects (see [Interacting with Shopify / REST via Liquid objects](../../core/shopify/#rest-via-liquid-objects)).
6. Optional: To bring over your email colors, go back to the "Notifications" area in Shopify, and click on the "Customize" button, under "Customer notifications". You'll see a setting for "Accent color". Click on the block of color, then copy the string of text that looks like "#ABCDEF". This is your color accent code. :) Back in your Mechanic email template, replace the two instances of "#1990c6" (the default accent color) with the color code that you copied, then save your settings.
7. Optional: To bring over your shop's logo, see our [How do I send images with my emails?](../../faq/how-do-i-send-images-with-my-emails.md) guide – see the "Embedding images" section.

### OrderlyEmails

If you're using OrderlyEmails, find the code for the "Blank" template, copy its code, and paste it in as your Mechanic email template.

On the display and formatting side of things, no further changes to the code are necessary – Mechanic will take care of making sure that the right variables are inserted. It may be necessary to update other Liquid variables for compatibility with Mechanic (see [Interacting with Shopify / REST via Liquid objects](../../core/shopify/#rest-via-liquid-objects)).
