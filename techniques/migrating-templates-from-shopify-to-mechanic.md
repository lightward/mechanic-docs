# Migrating templates from Shopify to Mechanic

You can bring your templates over from Shopify's email notifications, or from Shopify's [Order Printer](https://apps.shopify.com/order-printer) app, in order to generate order-related material on-demand in  Mechanic. This could look like [triggering an order confirmation email](../faq/can-i-re-send-order-confirmation-emails-with-mechanic.md), or [generating a PDF invoice](../core/actions/file-generators/pdf.md).

We'll talk about this in two parts: the task script, and then the steps required for preparing a Shopify template for use with Mechanic.

## Task script

To generate an order PDF to send as an email, you might use a task like this one, paired with a subscription to shopify/orders/create:

```text
{% action "email" %}
  {
    "to": {{ order.email | json }},
    "subject": {{ options.email_subject__required | strip | json }},
    "body": {{ options.email_body__multiline_required | strip | newline_to_br | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }},
    "attachments": {
      {{ options.pdf_attachment_filename__required | json }}: {
        "pdf": {
          "html": {{ options.pdf_html_template__code_multiline_required | json }}
        }
      }
    }
  }
{% endaction %}
```

Or, to send out an order-related email, you might use something like this:

```text
{% action "email" %}
  {
    "to": {{ order.email | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body_html__required_multiline_code | strip | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
```

Both task scripts offer the user a multi-line code field, allowing them to paste in the original template from Shopify.

A few template modifications are necessary before they'll function as intended, and we'll talk about that next.

## Preparing the template

Shopify and Mechanic both use Liquid code for their templates. Run through the following changes to convert your Shopify templates \(whether from an email notification or from Order Printer\) to something Mechanic can use.

### 1. Change variables to order properties

Shopify uses variables like `{{ total_price }}` and `{{ transactions }}` and `{{ email }}`. These are all properties of [the order object](../platform/liquid/objects/shopify/order.md), and you can access them as such in Mechanic, using variables like `{{ order.total_price }}` and `{{ order.transactions }}` and `{{ order.email }}`.

Some variables from Shopify are already given as properties of an object you can use in Mechanic. For example, `{{ shop.name }}` works in both Order Printer and in Shopify.

To see what variables Shopify uses in their notification templates, [see their documentation](https://help.shopify.com/en/manual/orders/notifications/email-variables). If you're using Order Printer, you can find a similar list by opening up a template in the app, and clicking the "View the Liquid variable list" link.

For a full list of order object properties in Mechanic, [see our documentation](../platform/liquid/objects/shopify/order.md).

### 2. Convert numeric strings to numbers, and check on money rendering

Mechanic uses API data from Shopify to power [the order object](../platform/liquid/objects/shopify/order.md). Because the Shopify API makes money figures available as strings, you may need to convert these values to numbers for parts of your template to work correctly.

Example:

```text
{% if total_price > 5000 %}
```

should become...

```text
{% assign total_price_number = order.total_price | times: 1 %} {% if total_price > 5000 %}
```

For numbers that are formatted with the "money" filter, check to make sure that the final numbers are correct. You may need to multiply the number by 100 to achieve the expected results, as in:

```text
{{ order.total_price | times: 100 | money }}
```

### 3. PDFs only: check your fonts

Under the hood, we use [wkhtmltopdf](https://wkhtmltopdf.org/) to generate PDFs. This engine does support web fonts, which includes fonts you may get from providers like [Google Fonts](https://fonts.google.com/). For an example, see this task: [Demonstration: Web fonts in PDFs](https://usemechanic.com/task/demonstration-web-fonts-in-pdfs)

In some cases, the code determining your font may need to be simplified. This varies, and is not always necessary; remember to check your fonts when testing, and send us a support message if you've got any questions.

### 4. Check your stylesheets

In general, external stylesheets are not supported. This means that any `<link rel="stylesheet">` tags need to be replaced with a `<style>` tag, containing the full contents of that stylesheet.

There is one exception. When using email templates, Mechanic will automatically replace this tag:

```text
<link rel="stylesheet" type="text/css" href="/assets/notifications/styles.css">
```

... with a style tag containing rules extracted from Shopify's default notification emails. You may still need to make overriding changes, particularly for brand colors, but this should help you make progress more rapidly.

{% hint style="warning" %}
Note: This &lt;link&gt; tag substitution only happens for email templates – it does not occur anywhere else.
{% endhint %}

