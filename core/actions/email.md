# Email

The **Email** action is for sending email. ✅ It supports the store's [**email templates**](../../platform/email/templates.md), and supports attachments constructed by [**file generators**](file-generators/).

## Restrictions

Mechanic sends email via [Postmark](https://postmarkapp.com), our email provider. Currently, Mechanic only supports Postmark's transactional message stream, which means that marketing and other bulk mail may not be sent. To learn more about what is and isn't a transactional message, see Postmark's article: ["What are Transactional emails?"](https://postmarkapp.com/support/article/804-what-are-transactional-emails).

## Options

| Option              | Description                                                                                                             |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `to`                | Required; an array or comma-delimited string of recipient addresses                                                     |
| `subject`           | Required; a string specifying the message subject                                                                       |
| `body`              | Required; an HTML string of body content; supports HTML and CSS                                                         |
| `cc`                | Optional; an array or comma-delimited string of cc addresses                                                            |
| `bcc`               | Optional; an array or comma-delimited string of bcc addresses                                                           |
| `reply_to`          | Optional; a single reply-to address                                                                                     |
| `from_display_name` | Optional; a string controlling the name (but not the address) of the sender                                             |
| `template`          | Optional; a string naming an email template from the current Mechanic account                                           |
| `attachments`       | Optional; an object specifying files to attach, using [file generators](file-generators/)                               |
| `...`               | Additional options may be provided, and will be made available to email templates as variables, named after each option |

## Message formatting

### HTML and CSS

Mechanic parses each email body for HTML and CSS, allowing authors to use `<style>` tags without having to think about email client compatibility.

{% hint style="info" %}
If you're simply trying to add formatted text and aren't ready to dig into the code yourself, try using a tool like [wordtohtml.net](https://wordtohtml.net) to quickly generate usable HTML.
{% endhint %}

### Embedded images

Images may be embedded using the `<img>` tag, but must be hosted independently. Shopify provides basic file hosting, appropriate for uploading images for use with Mechanic emails. To learn more, see [Uploading files to your website](https://help.shopify.com/en/manual/shopify-admin/productivity-tools/file-uploads).

## Changing the sender address

This action only supports sending from a single address (regardless of the sender name, as controlled by the `from_display_name` option).

By default, the sender address is a Mechanic address based on the store's myshopify.com subdomain. For example, the store example.myshopify.com will default to having its mail sent from example@mail.usemechanic.com.

Changing the sender address involves adding it to the store's Mechanic account, and then configuring the email domain name with some DNS records for verification.

For more on this, see [Custom email domain](../../platform/email/custom-email-domain.md).

## Template

To achieve easily reusable headers and footers, Mechanic can be configured with one or more email templates, available in the Mechanic account settings. To learn more about configuring email templates, see [Email templates](../../platform/email/templates.md).

To use a specific email template with the Email action, use the `template` option to specify the name of the desired email template.

### Creating email template variables

All options used with the Email action will be made available as Liquid variables for the email template. This means that standard options may be used, like `{{ subject }}` and `{{ body }}`, and also custom options: passing in an `"order_data"` option, containing order data, may allow the email template to show the order name via `{{ order_data.name }}`.

{% hint style="warning" %}
Note that custom options, like all task options, must be provided using standard JSON. This means that the data made available to email templates will be derived from plain JSON values.

For example, consider this action:

```javascript
{% raw %}
{% action "email" %}
  {
    "to": "customer@example.com",
    "subject": "Thanks for your order!",
    "template": "order_acknowledgement",
    "order_data": {{ order | json }}
  }
{% endaction %}
{% endraw %}
```

The template named "order\_acknowledgement" could include the following Liquid, and get the expected results:

```javascript
This is the first item: {{ order_data.line_items.first.title }}
```

But, because `order_data` is a plain [hash](../../platform/liquid/keyword-literals/hash.md) based entirely on JSON data, instead of being an enhanced order object (see [Environment variables](../tasks/code/environment-variables.md)), the following Liquid usage would fail:

```javascript
Remember order {{ order_data.customer.orders.any.first.number }}, your first ever?
```
{% endhint %}

## Attachments

This action supports attachments given in Mechanic's file generator format. This structure allows the sender to construct a variety of files, including ad-hoc text-based files, PDFs rendered from HTML, files dynamically downloaded from external locations, and ZIP files containing any other files.

For more on this, see [File generators](file-generators/).

## Examples

### Simple

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": "It's a mighty fine day!",
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "email",
    "options": {
      "to": "hello@example.com",
      "subject": "Hello world",
      "body": "It's a mighty fine day!",
      "reply_to": "store@example.com",
      "from_display_name": "Example Store"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Using HTML

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% capture email_body %}
  <b>Hello!</b>

  It's fantastic to see you!
{% endcapture %}

{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": {{ email_body | unindent | strip | newline_to_br | json }}
  }
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "email",
    "options": {
      "to": "hello@example.com",
      "subject": "Hello world",
      "body": "<b>Hello!</b><br />\n<br />\nIt's fantastic to see you!"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Using attachments

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% raw %}
{% action "email" %}
  {
    "to": "test@example.com",
    "subject": "An image test",
    "body": "Please see attached. :)",
    "attachments": {
      "a_configured_image_from_the_web.png": {
        "url": "https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png"
      }
    }
  }
{% endaction %}
{% endraw %}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "action": {
    "type": "email",
    "options": {
      "to": "test@example.com",
      "subject": "An image test",
      "body": "Please see attached. :)",
      "attachments": {
        "a_configured_image_from_the_web.png": {
          "url": "https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
