# Email templates

A Mechanic account may be configured with one or more **email templates**, used by the [**Email action**](../../core/actions/email.md) to render email content using a Liquid that can be shared across tasks. Email templates have access to Liquid [**template variables**](../../core/actions/email.md#creating-template-variables) named after each option in the Email action, including any custom options added by the task author.

Unless configured otherwise, each email will use the email template named "default", if it exists.

## Configuration

Navigate to your Mechanic settings (it's the "Settings" link in the upper right), and head to the "Email templates" tab. Click the "New email template" link to start editing your new template.

![](<../../.gitbook/assets/Screen Shot 2022-02-27 at 10.53.14 AM.png>)

{% hint style="info" %}
To learn more about formatting messages with HTML, CSS, and images, see [Message formatting](../../core/actions/email.md#message-formatting).
{% endhint %}

{% hint style="info" %}
To pass custom variables along to the email template, specify them as additional options to the Email action. To learn more about this technique, see [Creating template variables](../../core/actions/email.md#creating-template-variables).
{% endhint %}

## Specifying a template

Mechanic will always default to using the template named "default", when present. Feel free to add additional templates – just remember to update your task(s) to use the appropriate template.

Here's an example email action, specifying a non-default template:

```cpp
{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": "It's a mighty fine day!",
    "template": "welcome"
  }
{% endaction %}
```

## Migrating from Shopify

Shopify's notification templates may be manually migrated over to Mechanic. To learn more, see [Migrating templates from Shopify to Mechanic](../../techniques/migrating-templates-from-shopify-to-mechanic.md).
