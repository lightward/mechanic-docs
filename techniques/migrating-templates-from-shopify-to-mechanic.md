# Migrating templates from Shopify to Mechanic

Shopify notification templates can be manually migrated over to Mechanic in order to generate order-related material on-demand in Mechanic. This could look like [triggering an order confirmation email](../faq/can-i-resend-order-confirmation-emails-with-mechanic.md), or [generating a PDF invoice](../core/actions/file-generators/pdf.md).

Common sources for these templates include Shopify's email notification settings, Shopify's [Order Printer](https://apps.shopify.com/order-printer) app, and [OrderlyEmails](https://apps.shopify.com/email-templates).

We'll talk about this in two parts: the task code, and then the steps required for preparing a Shopify template for use with Mechanic.

Before we begin, note that there are two ways to think about templates:

* Reusable email templates, as we'll call them here, are configured in Mechanic's "Email templates" settings area. Here, email templates may be keyed with a name and saved for later use and reuse by any number of tasks.&#x20;
* Task-level templates are ad-hoc pieces of Liquid code, informally defined within an individual task, and possibly made user-configurable via [task options](../core/tasks/options/). While this page focuses on using templates for email purposes, a task-level template may be used for more than just emails: a task might also use these to generate PDFs, or file content to save via FTP, or (in very advanced cases) even entire [action objects](../core/tasks/code/action-objects.md).

## Task code

There are several ways your task code might be set up for exercising a template.

{% hint style="warning" %}
In any of these scenarios, your Shopify template will need to be modified before it'll function as intended in Mechanic, and we'll talk about that below in [Preparing the template](migrating-templates-from-shopify-to-mechanic.md#preparing-the-template).
{% endhint %}

### Generating a PDF with a template from task options

To generate an order PDF to send as an email, you might use a task like this one, paired with a subscription to shopify/orders/create. Note how the template is made user-configurable by a task option, implicitly created by the `options.pdf_html_template__code_multiline_required` variable reference.

```
{% raw %}
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
{% endraw %}
```

### Generating email body HTML using a template from task options

To send out an order-related email where all the details are provided as HTML in the email itself, you might use something like this. Note how, as above, the template is set using task options.

```
{% raw %}
{% action "email" %}
  {
    "to": {{ order.email | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body_html__required_multiline_code | strip | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }}
  }
{% endaction %}
{% endraw %}
```

### Generating email body HTML from a reusable email template

To send an email using a reusable [email template](../platform/email/templates.md), as configured in your Mechanic settings, use the `"template"` option of the [Email action](../core/actions/email.md).

{% hint style="info" %}
You may pass along additional variables to your email template, by using your own custom action options. In the example below, we pass along two custom options, with two different techniques:

* By specifying `"order_data"`, we allow the email template to use a variable called `order_data` which includes a copy of everything we know about the current order. The email template will be able to call values from this variable using something like `{{ order_data.name }}`.
* By specifying `"additional_data"`, we define our own static values for a new email template variable called `additional_data`. The order template will be able to call values from this variable using something like `{{ additional_data.some }}`.

To learn more about this, see [Creating email template variables](../core/actions/email.md#creating-email-template-variables).
{% endhint %}

```
{% raw %}
{% action "email" %}
  {
    "to": {{ order.email | json }},
    "subject": {{ options.email_subject__required | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }},
    "template": "some_template_name",
    "order_data": {{ order | json }},
    "additional_data": {
      "some": "additional values"
    }
  }
{% endaction %}
{% endraw %}
```

## Preparing the template

Shopify and Mechanic both use Liquid code for their templates. Run through the following changes to convert your Shopify templates (whether from a Shopify email notification or from the Order Printer app) to something Mechanic can use.

### Update variables to reference order properties

Shopify uses variables like `{{ total_price }}` and `{{ transactions }}` and `{{ email }}`. These variables aren't automatically available in Mechanic, but the right pieces of data all available via [the Order object](../platform/liquid/objects/shopify/order.md), using references like `{{ order.total_price }}` and `{{ order.transactions }}` and `{{ order.email }}`.

Rather than changing all of these variable references across the template, it may be easier to initialize those variables at the top of the template code, using something like this:

```
{% raw %}
{% assign total_price = order.total_price %}
{% assign transactions = order.transactions %}
{% assign email = order.email %}
{% endraw %}
```

Some variables from Shopify are already given as properties of an object you can use in Mechanic. For example, `{{ shop.name }}` works in both Order Printer and in Shopify.

To see what variables Shopify uses in their notification templates, [see their documentation](https://help.shopify.com/en/manual/orders/notifications/email-variables). If you're using Order Printer, you can find a similar list by opening up a template in the app, and clicking the "View the Liquid variable list" link.

{% hint style="info" %}
Mechanic's usual sources for querying Shopify data may be used here as well. To learn more about pulling in Shopify data, see [Reading data](../core/shopify/read/).
{% endhint %}

### Check for Liquid filter support

Mechanic Liquid supports many filters from Shopify Liquid, but not all of them! If a filter isn't working the way you expect, check [Mechanic's list of supported Shopify Liquid filters](broken-reference). You may need to adjust your code to work around filters that Mechanic Liquid doesn't support.

### Convert numeric strings to numbers, and check on money rendering

Mechanic uses API data from Shopify to power [the Order object](../platform/liquid/objects/shopify/order.md). Because the Shopify API makes money figures available as strings, one _may_ need to cast these values to numbers for parts of your template to work correctly. This may be done via assignment, as in `{% assign foo = some_number | times: 1 %}`, or at the time of output, as in `{{ some_number | times: 1 }}`.

This is particularly important if the figure is used in a logical condition, like this one:

```
{% raw %}
{% if order.total_price > 5000 %}
{% endraw %}
```

Because `order.total_price` is a string, and because Liquid filters cannot be used in the middle of an `if` or `unless` tag, one should first assign the numeric value to a new variable and then update the condition to use that new variable.

```
{% raw %}
{% assign total_price_number = order.total_price | times: 1 %}
{% if total_price > 5000 %}
{% endraw %}
```

For numbers that are formatted with the "money" or "money\_with\_currency" filter, check to make sure that the final numbers are correct. You may need to multiply the number by 100 to achieve the expected results, as in:

```
{{ order.total_price | times: 100 | money }}
{{ order.total_price | times: 100 | money_with_currency }}
```

### Check your stylesheets

External stylesheets are not supported for emails (though they _are_ supported for the [PDF file generator](../core/actions/file-generators/pdf.md)). This means that, for email content, any `<link rel="stylesheet">` tags need to be replaced with a `<style>` tag, containing the full contents of that stylesheet.

There is one exception scenario. When using email templates, Mechanic will automatically replace this tag:

```
<link rel="stylesheet" type="text/css" href="/assets/notifications/styles.css">
```

... with a style tag containing rules extracted from Shopify's default notification emails. You may still need to make overriding changes, particularly for brand colors, but this should help you make progress more rapidly.

{% hint style="warning" %}
Note: This \<link> tag substitution only happens for reusable email templates. It does not occur in ad-hoc templates defined at the task level, or anywhere else.
{% endhint %}

### Updating branding

To bring over your email colors, go back to the "Notifications" area in Shopify, and click on the "Customize" button, under "Customer notifications". You'll see a setting for "Accent color". Click on the block of color, then copy the string of text that looks like "#ABCDEF". This is your color accent code. :) Back in your Mechanic email template, replace the two instances of "#1990c6" (the default accent color) with the color code that you copied, then save your settings.

To bring over your shop's logo, see our [How do I send images with my emails?](../faq/how-do-i-send-images-with-my-emails.md) guide – see the "Embedding images" section.
