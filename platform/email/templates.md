---
description: Reuse an existing email template, edit its content and branding, send a test, and use it in your Mechanic tasks.
---

# Email templates

Give your task emails the same logo, colors, and layout as the emails your store already sends. Create a reusable template in **Settings → Email templates**, then use it with the [Email action](../../core/actions/email.md).

The template supplies the layout. The task supplies the recipient, subject, and message, including any calculated order or product details. Several tasks can share one template.

## Configuration

Open **Settings → Email templates**, then choose **New email template**. Give it a name you'll recognize when choosing it in a task.

When the visual editor is available for your shop, opening or creating a template opens a full-width workspace. Use the back arrow to return to **Email templates**. On desktop, the content list, preview, and editing controls appear together; smaller screens use panel buttons.

New templates open in the visual editor automatically. You can import an existing email or arrange sections in a new layout without writing HTML. Existing HTML/Liquid templates keep their code editor.

### Start with an email you already send

Forwarding an existing email is the quickest way to get a familiar look:

1. Choose **Use an existing email** on a new template. To replace a saved template, choose **Template actions → Replace from email**.
2. Forward one store email to the temporary address shown in the dialog. Use this address, rather than your shop's usual Mechanic incoming email address.
3. Keep **Keep the original template** selected. Review the original beside the reusable template, where a sample message replaces the original message area.
4. Check that no customer or order details remain in the reusable header or footer, then choose **Use this template**. You can edit its text, images, links, and colors before saving.

The address expires after 30 minutes. If it expires, start again to get a new one. These imports are separate from normal [incoming email](receiving-email.md): they do not trigger tasks.

If forwarding isn't available, or you already have a saved copy, choose an `.eml` or `.html` file in the same dialog. Imports can be up to 5 MB.

For supported Shopify-style emails, importing preserves the original table layout, styling, responsive rules, and supported Outlook wrappers. The original message and order sections are replaced by the task message. Original link destinations are removed so private order or unsubscribe links aren't reused; add the links you want in the editor. Always review the remaining header and footer. Forwarding and importing never save automatically.

Some emails don't have a recognizable message area, and some use custom markup the importer can't preserve. In that case, it tells you. Try forwarding the original as an attachment, use the HTML editor, or deliberately choose **Rebuild the look with editable blocks**. That option creates a new layout using the source's colors, fonts, width and selected logo; it doesn't preserve the original HTML.

Images in the original preview load only when you choose to load them. Loading them contacts the original image host.

### Edit the layout

An imported template offers **Content and colors** controls for its text, images, image descriptions, links, and supported colors. These edits keep the surrounding layout intact. Use **Edit code** for structural changes. Opening code and returning with **Edit visually** keeps the same email. Supported code edits return as a preserved layout with content and color controls; they are not rebuilt as blocks. If code uses unsupported markup or Liquid, it stays in the code editor without losing your changes.

**Template actions → Start a new layout** replaces the current draft after confirmation.

Code editing also has a live, read-only preview. It updates as you type without changing your code. On smaller screens, use **Code** and **Preview** to switch panels. This works for existing HTML templates too, even when their code cannot be edited visually.

The preview uses a sample task message and leaves other Liquid unevaluated. Use the task's email preview to see its actual values. Some email-client-specific markup may look different in the browser; check a delivered test before relying on the final appearance.

In a block layout, add and arrange text, images, buttons, dividers, and spacing around the **Task message** section. This section marks where the task's message will appear. You can move it, but it must remain in the layout.

The sample message helps you judge the layout. It is replaced by the task's actual message when the task sends an email. Visual sections don't accept Liquid variables or repeating product lists; calculate those details in the task and include them in its message. HTML/Liquid templates remain available for custom template variables.

### Send a test email

Choose **Send test email** to see the current imported or block-layout draft in your inbox before saving it.

1. Check the recipient. It starts with your signed-in staff email address. Before your shop is approved for sending email, tests can only go to that address. Approved shops can choose one other recipient.
2. Send the test, then look for **[Test] Email template** in that inbox.
3. Review the layout and images, adjust the draft, and save when you're happy with it.

The test uses a fixed sample message and your shop's configured sender. It includes your unsaved layout changes, but does not save the template or run a task. Up to ten template tests per shop per hour are allowed, in addition to the usual email sending limits.

A template test sent to your signed-in staff email does not require shop email approval. Normal task emails still require approval, and shop suspension, trial, fraud, and rate-limit restrictions still apply.

Check it in the email clients your recipients use. Fonts, rounded corners, and dark-mode colors can vary between clients; a browser preview can't establish inbox appearance.

To check a task's actual calculated message, use that task's email preview and **Send a copy**. Neither preview copies nor layout tests include attachments; see [How do I preview email attachments?](../../faq/how-do-i-preview-email-attachments.md).

If sending reports a problem, check your inbox before retrying: an uncertain response can occur after a message has been accepted for delivery.

### Images and Shopify Files

For an imported image, image section, or logo, you can:

* **Choose from Shopify Files** to reuse an image from your shop. Shopify's picker also offers uploads.
* **Upload to Shopify Files** using the editor's separate upload control.
* Paste an existing public HTTPS image URL.

When prompted, choose **Enable Shopify Files**, approve access in the new tab, then return to the editor and choose **Check access**. If Mechanic already has Files access, another approval isn't needed. Pasting a hosted image URL doesn't require Files permission.

The separate upload control accepts PNG and JPEG files up to 2 MB and 4096 × 4096 pixels, with up to 20 upload attempts per shop per hour. Uploads inside Shopify's picker follow Shopify's own limits. Shopify hosts and processes the images; Mechanic requests a version up to 1200 pixels wide, without guaranteeing a particular file size.

Manage uploaded images in Shopify **Content → Files**. They are public files linked from the email, not email attachments. Deleting a template doesn't delete its images. Deleting a file can break images in emails you've already sent.

If you copy a template to another shop, its image URLs stay the same; the files aren't copied. Upload your own copy in the destination shop if you want that shop to control the images. See [How do I send images with my emails?](../../faq/how-do-i-send-images-with-my-emails.md) for images in task code and attachments.

## Specifying a template

### Choose a template in a task

If a task offers an **Email template** option, choose your saved template there. The picker lets you preview or open a template, or create/import one in a separate tab. After saving a new template, return to the task and choose **Refresh templates**.

**Preview template** shows the saved layout with sample content. It does not evaluate Liquid. Use the task's email preview to check its calculated content with the chosen template.

Start with [Demonstration: Send an email using a saved template](https://tasks.mechanic.dev/demonstration-send-an-email-using-a-saved-template). It sends one message when you run it manually, and shows how the task's message fits inside the saved layout.

Existing library tasks keep their current options and behavior. An email subject or body field doesn't automatically gain a template picker. A task author can [add one explicitly](../../core/tasks/options/README.md#email-template-picker).

In tasks with an optional picker, **Keep current behavior** leaves the task's existing template choice in place. Saving or importing a template doesn't change any task's selection.

### Select a template in code

Use the Email action's `template` option to choose a saved template by name:

```liquid
{% action "email" %}
  {
    "to": "hello@example.com",
    "subject": "Hello world",
    "body": "It's a mighty fine day!",
    "template": "welcome"
  }
{% endaction %}
```

If `template` is omitted or `null`, Mechanic uses the template named `default`, if one exists. Set `"template": false` to send the body without a template. A missing named template other than `default` causes an error.

Each email action can choose its own template. A task with several email actions can share one picker, or expose separate pickers for different messages.

Template references use names. Renaming a template does not update tasks that use it, so update those references as part of a rename. Editing a saved template changes the layout for future emails that select it.

## Existing HTML/Liquid templates

Existing templates continue to work and can still be edited as HTML/Liquid. They have access to [template variables](../../core/actions/email.md#creating-email-template-variables) named after each Email action option, including custom options supplied by the task author.

<figure><img src="../../.gitbook/assets/mechanic-email-templates.png" alt="Editing an HTML email template in Mechanic settings"><figcaption></figcaption></figure>

**Edit visually** opens supported HTML as a preserved layout. Code that the visual editor cannot preserve stays in the code editor. **Template actions → Start a new layout** separately replaces the current draft after confirmation. The saved template stays unchanged until you save.

To test an HTML/Liquid template with task data, use the task's email preview and **Send a copy**. The template-page **Send test email** control is for imported and block layouts.

For formatting task messages with HTML and CSS, see [Message formatting](../../core/actions/email.md#message-formatting).

## Migrating from Shopify

Start by forwarding or importing an existing email to reuse its supported layout. Importing doesn't recover Shopify's original Liquid logic or turn old order details into live fields. To reproduce full notification logic or create a PDF from a Shopify template, see [Migrating templates from Shopify to Mechanic](../../techniques/migrating-templates-from-shopify-to-mechanic.md).
