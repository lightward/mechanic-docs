---
description: Build storefront forms that send answers to your Mechanic tasks through webhooks.
---

# Forms

Build your form in Mechanic, then add it to a page in your theme. When someone submits it, the form sends their answers to the [Mechanic webhook](../platform/webhooks.md) you chose. Mechanic picks up the queued request and creates its event. Your subscribed tasks handle what happens next.

## Build your form

1. Open **Forms** and choose **Create form**.
2. Choose a starter with **Use template**, **Start from scratch**, or **Import JSON**. A template creates an unpublished draft, ready to edit. **Preview** lets you try its questions and steps without saving or sending your answers.
3. Add fields, then select each field to edit its label and settings. Reorder them with the drag handles or move controls.
4. Set the heading, introduction, and button text. **Form name** is only shown inside Mechanic.
5. Choose **Try form** to try the questions and validation. This preview does not send submissions or run tasks.
6. Save your draft. You can return to it before publishing.

There are seven starters: **Warranty request**, **Service or repair request**, **Request a quote**, **Wholesale application**, **Customization request**, **Customer feedback**, and **Basic form**. Longer starters have two or three steps; some include conditional questions and optional uploads. Every field and message is editable. Choose your own webhook in Submission settings before publishing.

Templates provide questions, not an approval or booking workflow. Your subscribed tasks handle each request. Changes to the starter library do not overwrite forms you have already created.

### Choose a layout

Under **Form layout**, choose **Grouped steps** to show the questions in each step together, or **One question at a time** to guide visitors through individual questions. Address fields and multiple-choice options stay together. Both layouts support conditional questions and file uploads.

Use **Try form** to check the flow. Visitors can go Back without losing their answers or selected files. Nothing is submitted until the final Send button. Save and publish to update the layout in your theme; existing forms keep their current layout until you change it. The layout also travels with JSON exports.

Your theme supplies the form’s fonts and colors. You do not need to edit theme code or write storefront JavaScript to use a form block.

## Connect your tasks

Open **Submission settings** and choose a webhook under **Send submissions to**. Its event topic determines which tasks receive the submission. The form does not have a separate event topic.

Choose **Create or manage webhooks** to open webhook settings in a separate tab. [Create the webhook](../resources/tutorials/creating-a-mechanic-webhook.md), return to your form, and choose **Refresh connection** to select it. Your form edits stay in the original tab.

**Tasks subscribed to this topic** lists the connected tasks, with links to open them. Disabled tasks are marked and will not run. If the list is empty, add the webhook’s topic to the subscriptions of a task you want to run. One webhook can serve several forms and tasks.

Write a **Confirmation message** such as “Thanks! We received your request.” Tasks run in the background, so the message should confirm receipt without promising that a task has already finished.

For task authors, answers appear under `event.data.fields`, using each field’s data key. Full-request webhooks use `event.data.body.fields`. The payload also identifies the form and submission. Keep data keys stable once tasks depend on them; labels can change independently. These are ordinary multipart form submissions: scalar answers are strings, including numbers and checkbox values (`"true"` or `"false"`). Checkbox groups are arrays; empty groups and unselected files are omitted. Treat all submitted data as visitor input and validate what your task needs.

## Save your submissions

After choosing a webhook, you can use these tasks on its event topic:

- [Google Sheets task](https://tasks.mechanic.dev/save-mechanic-form-submissions-to-a-google-sheet): save answers as spreadsheet columns, with an option to upload files to Google Drive and save their links. Files are not automatically shared publicly.
- [Email task](https://tasks.mechanic.dev/email-mechanic-form-submissions): send answers to your team, with an option to include uploaded files as attachments.
- [Shopify metaobjects task](https://tasks.mechanic.dev/save-mechanic-form-submissions-to-shopify-metaobjects): save all answers in a standard Shopify entry, with storefront access off.

Install the tasks you need; they can run together. Copy the webhook's **Event topic** into the task's **Webhook event topic** option. To handle just this form, copy its form code into **Form ID**; otherwise the task handles all Mechanic forms on that topic. Complete the task's destination setup, save and enable it, then return to the form and refresh the connection. The task should appear in the subscribed-task list. Installing a task does not reconnect the form or complete its setup automatically.

**Choose what happens to uploaded files.** The Google Sheets task can save them to Google Drive when its upload option is enabled, and the email task can include them as attachments. Without those options, you receive file details only. The metaobject task stores answers and file details, not the files themselves. Files in the original Mechanic event follow its normal retention.

### Example: a warranty request

1. Create a form from **Warranty request** and select a webhook in Submission settings.
2. Install the **Google Sheets task**. Connect your Google account under Mechanic's Settings → Authentication, then set the account and spreadsheet title in the task. Configure columns using the warranty form's data keys: `Name → name`, `Email → email`, `Product → product`, and `Request → issue`. Keep `receipt` and `photo` out of that mapping; their details appear in the fixed File details column.
3. Save and run the task manually with Spreadsheet ID blank. Copy `spreadsheet_id` from the completed setup action into the task and save again. The sheet must be created through Mechanic. Headers and configured column order must stay aligned.
4. Alternatively, install the **Shopify metaobjects task**, set Form name to Warranty request, grant its requested permissions, and run it manually once. Wait for the definition setup action to succeed. Records will appear in Shopify under **Content → Metaobjects → Mechanic form submission**. Answers are stored as JSON so changing questions does not require a new definition.
5. Enable the task, publish/place the form, and submit a sample from the actual storefront. Check both the task's action result and the saved row or entry. A form confirmation alone does not establish a successful save.

Google Sheets appends may duplicate a row when an event is rerun or a visitor resends. The task includes IDs for finding repeats and disables ambiguous-write retries; inspect the sheet before manually retrying a failed append. The metaobject task updates the same entry when the same Mechanic event is rerun. A new HTTP submission creates a new event and a separate entry, even with the same browser submission ID.

When **Include uploaded files** is off in the email task, the message contains file names and sizes, with no download links. Turn it on to receive the files as attachments, or use the Google Sheets task’s Google Drive upload option for lasting storage. Metaobject submission records store file details, not the uploaded files. Files in the original event follow Mechanic’s normal event retention; Forms does not provide separate file hosting or expiring download links.

## Add the form to your theme

1. Save your draft, then choose **Publish form**. Mechanic opens the **Theme** tab, which you can return to at any time.
2. Choose **Add to home page**, or **Open theme editor** to choose another template.
3. Add or select the **Mechanic form** block. Choose your published form in its **Form** picker and place the block where you want it.
4. Click **Save** in the theme editor.
5. Open the storefront page in a separate tab and send a test submission. Check the resulting event and task runs in Mechanic.

The link opens the editor; it does not save the theme or select a form for you. Your theme must support app blocks at the chosen location. The same form can have several placements. Older blocks with a form code keep working. Clear an existing code before switching that block to the picker. If the picker is unavailable, Mechanic provides form-code instructions.

### Where is this form used?

In the **Theme** tab, **Where this form is used** checks saved placements in your live theme. Choose another theme to check its placements, including unpublished themes. Each result identifies the template or section group and links to the theme editor. Hidden blocks are marked. A template may serve several pages; the list does not enumerate individual page URLs.

Saving a form adds permission to read your themes to Mechanic’s required access. If an update is needed, choose **Update access** and approve the Shopify permission request. This lets Mechanic find where your forms are placed without changing theme files. You can see **Forms** listed as the reason on Mechanic’s Permissions page. Deleting your last form removes this requirement unless a task or another feature still needs it. Your forms continue working while you complete the step. After granting access, return to the form’s Theme tab.

Choose **Refresh** after saving a theme. Results can take up to 30 seconds to update. Unsaved editor changes are not included. Blocks that choose their form through connected data can vary by page and cannot be attributed to one form; Mechanic explains when a check includes those blocks. A failed check is not evidence that the form is unused.

### Test without surprises

The preview inside Mechanic and the form inside Shopify’s theme editor do not send submissions or run tasks. **A storefront opened in a separate tab, including an unpublished theme preview, sends real submissions and can run tasks.** Use a test webhook and test destinations when trying delivery.

## Steps, conditions, and files

Use **Manage steps** to name and arrange up to 10 steps, then use each field’s **Step** setting to place it. Visitors keep their answers and selected files when moving between steps. Only the final submit button sends the form; partial answers are not sent after each step.

Use **Show this field** to ask a question based on an earlier answer. Hidden questions are not required, and their old answers or files are left out of the submission. Empty conditional steps are skipped. If reordering a question leaves a condition needing attention, correct it before saving.

**File upload** accepts one file per field. Choose allowed extensions and a per-file limit from 1 to 3 MB. All files together must fit within **3 MB per submission**. Tasks receive the name, type, size, and base64 contents in the normal webhook file format; no separate file hosting is needed.

Other fields include text, email, phone, website, numbers, dates, time, date and time, addresses, dropdowns, radio choices, single and multiple checkboxes, ratings, and headings. Time values represent the time entered by the visitor and do not include a timezone.

## Copy a form to another shop

Choose **Export JSON** to download the current form, including any valid unsaved edits. Fields, data keys, settings, steps, conditions, and text are included. Answers, files, tasks, webhook credentials, and theme placements are not included.

In the destination shop, open **Forms → Import form** and upload or paste the JSON. Review it, select that shop’s webhook or **Choose later**, then choose **Import as draft**. The new form starts unpublished and gets a new form code. It needs a webhook before publishing.

Set up the destination shop’s webhook and subscribed tasks separately. Publish the form and select it in that shop’s theme. Importing does not reconnect the form to the original shop.

## Update or stop a form

Saving edits updates the draft. Visitors keep seeing the published version until you choose **Publish changes**. Published changes apply everywhere that form is placed.

Choose **Form actions → Unpublish form** to stop showing the form to new visitors. Someone with the form already open can still send it, and the reusable webhook stays available. Disabling or deleting the webhook stops it from creating events, but affects every form or integration using that webhook. Previously received events continue through the normal task queue.

Disabling a task stops that task from running; it does not unpublish the form. Other enabled tasks subscribed to the same webhook can still run. Unpublish the form and remove its block from the theme when it is no longer needed.

If a visitor loses the confirmation, their answers stay on screen. **Send again** warns that the earlier submission may have arrived and another attempt could send it twice. The form does not retry automatically. The normal webhook acknowledgment confirms receipt at the webhook service; it does not confirm a task ran, and it also acknowledges disabled or invalid webhook URLs.
