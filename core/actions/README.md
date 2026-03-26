---
description: "Actions are how Mechanic tasks make changes \u2014 Shopify mutations, emails, HTTP requests, file generation, and more."
---

# Actions

An **action** is an operation with an effect: a Shopify API mutation, an HTTP request, an email, a generated file, or a custom event. Tasks produce actions by rendering JSON action definitions in Liquid, which means a single task can dynamically generate any number of actions based on the data it processes.

Actions run after the task script completes. Each action has a **type** (specifying the operation) and **options** (providing specifics). Action definitions can be constructed using the [**action tag**](../../platform/liquid/tags/action.md) or as raw [**action objects**](../tasks/code/action-objects.md) in JSON.

{% hint style="info" %}
In Mechanic, actions are performed after their originating task run concludes. Actions are not performed inline during the task's Liquid rendering.

To inspect and respond to the results of an HTTP action, add a task subscription to mechanic/actions/perform, allowing the action to re-invoke the task with the action result data.

If you don't need the follow-up event for a particular action, add `__perform_event: false` to that action to skip emitting mechanic/actions/perform while still performing the action.

To attach meta while using any of the action tag syntaxes, add `__meta` and Mechanic will move that value into the action's meta field.

Learn more: [Responding to action results](../../techniques/responding-to-action-results.md)
{% endhint %}

## Action types

An **action type** determines the class of operation to be performed. While actions may vary greatly, there are only a few action types. Shopify is treated as a first-class action because it's the primary API Mechanic automates against.

| Action            | Purpose                                                                              |
| ----------------- | ------------------------------------------------------------------------------------ |
| [Cache](cache.md) | Performing operations on the store's Mechanic cache                                  |
| [Echo](echo.md)   | Debugging; displays the options that it is provided, with no side-effects            |
| [Email](email.md) | Sending transactional email                                                          |
| [Event](event.md) | Generating custom events in the [User event domain](../../platform/events/topics.md#user) |
| [Files](files.md) | Generating files of various types, storing them at a temporary Mechanic-provided URL |
| [FTP](ftp.md)     | Performing FTP file uploads and downloads                                            |
| [HTTP](http.md)   | Performing HTTP requests                                                             |
| [Shopify](shopify.md) | Sending requests to the Shopify Admin API                                      |

### Integrations

Mechanic also maintains a set of integration actions for third-party services beyond Shopify.

| Action                                           | Integrated service               | Purpose                                                                        |
| ------------------------------------------------ | -------------------------------- | ------------------------------------------------------------------------------ |
| [Airtable](integrations/airtable.md)             | Airtable                         | Create and edit tables and records in Airtable                                 |
| [Flow](integrations/flow.md)                     | Shopify Flow                     | Sending customer, order, product, and general triggers to Shopify Flow         |
| [Google](integrations/google.md)                 | Google (Drive and Google Sheets) | Advanced integration with the Google Drive and Sheets APIs                     |
| [Google Drive](integrations/google-drive.md)     | Google Drive                     | Upload files to Google Drive                                                   |
| [Google Sheets](integrations/google-sheets.md)   | Google Sheets                    | Create, Update, Export Google Sheets                                           |
| [Report Toaster](integrations/report-toaster.md) | Report Toaster                   | Requesting reports from Report Toaster, or updating data within Report Toaster |
| [Slack](integrations/slack.md)                   | Slack                            | Posting messages to Slack                                                      |
