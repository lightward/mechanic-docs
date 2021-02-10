# Email

The **Email** action is for sending email. ✅ It supports the store's [**email templates**](../../../advanced-topics/email-templates.md), and supports attachments constructed by [**file generators**](../file-generators.md).

## Usage restrictions

Mechanic sends email via [Postmark](https://postmarkapp.com/), our email provider. Currently, Mechanic only supports Postmark's transactional message stream, which means that marketing and other bulk mail may not be sent. To learn more about what is and isn't a transactional message, see Postmark's article: ["What are Transactional emails?"](https://postmarkapp.com/support/article/804-what-are-transactional-emails).

## Options

| Option | Description |
| :--- | :--- |
| `to` | Required; an array or comma-delimited string of recipient addresses |
| `subject` | Required; a string specifying the message subject |
| `body` | Required; an HTML string of body content; supports HTML and CSS |
| `cc` | Optional; an array or comma-delimited string of cc addresses |
| `bcc` | Optional; an array or comma-delimited string of bcc addresses |
| `reply_to` | Optional; a single reply-to address |
| `from_display_name` | Optional; a string controlling the name \(but not the address\) of the sender |
| `template` | Optional; a string naming an email template from the current Mechanic account |
| `attachments` | Optional; an object specifying files to attach, using [file generators](../file-generators.md) |

## Changing the sender address

This action only supports sending from a single address \(regardless of the sender name, as controlled by the `from_display_name` option\).

By default, the sender address is a Mechanic address based on the store's myshopify.com subdomain. For example, the store example.myshopify.com will default to having its mail sent from example@mail.usemechanic.com.

Changing the sender address involves adding it to the store's Mechanic account, and then configuring the email domain name with some DNS records for verification.

For more on this, see [Custom email domain](../../../advanced-topics/custom-email-domain.md).

## Attachments

This action supports attachments given in Mechanic's file generator format. This structure allows the sender to construct a variety of files, including ad-hoc text-based files, PDFs rendered from HTML, files dynamically downloaded from external locations, and ZIP files containing any other files.

For more on this, see [File generators](../file-generators.md).

## Examples

### Simple

{% tabs %}
{% tab title="Liquid" %}
```javascript
{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": "It's a mighty fine day!",
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
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

### 

