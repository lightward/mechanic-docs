---
description: Create, edit, and test Mechanic tasks with the built-in code editor.
---

# Task editor

The task editor is where you write and configure Mechanic [tasks](../core/tasks/). It has two modes: **Basic** for simple configuration, and **Advanced** for full access to code, preview events, JavaScript, runtime settings, permissions, and documentation.

Switch between modes using the **Advanced/Basic mode** toggle in the page actions menu. You can set your default mode in [Settings](settings.md).

## Basic mode

<figure><img src="../.gitbook/assets/basic_task_mechanic.jpg" alt="The task editor in Basic mode, showing options and documentation"><figcaption></figcaption></figure>

Basic mode shows the task's [options](../core/tasks/options/) and documentation. This is the default view for merchants using pre-built tasks who don't need to edit code.

## Advanced mode

<figure><img src="../.gitbook/assets/advanced_task_mechanic.jpg" alt="The task editor in Advanced mode, showing the code tab with subscriptions, Liquid editor, options sidebar, and preview"><figcaption></figcaption></figure>

Advanced mode provides a tabbed editor with a sidebar showing **Options** and a live **Preview** of the actions your task will generate.

### Code tab

The main editing surface:

* **Subscriptions** — define which [event topics](../core/events/topics.md) trigger this task, one per line
* **Code** — write the task's [Liquid script](../platform/liquid/)

### Preview events tab

Create test events to see how your task will behave. The task [preview](../core/tasks/previews/) updates automatically as you edit, showing generated actions, errors, and any Shopify permission changes.

### JavaScript tab

Add JavaScript that runs in your customers' browser (on the online storefront or order status page). Supports Liquid templating within the JavaScript.

### Runtime tab

* **Shopify Admin API version** — which [API version](../core/shopify/api-versions.md) the task uses
* **Perform action runs in sequence** — execute actions one at a time instead of in parallel
* **Halt the sequence when one fails** — stop processing remaining actions if one fails

### Permissions tab

A read-only view of the Shopify API [permissions](../core/tasks/permissions.md) your task requires, automatically detected from your code and preview.

### Docs & Tags tab

Add tags to organize your task, and write Markdown documentation with a live preview.

## Page actions

The actions menu in the page header includes:

* **Events** — view all events processed by this task
* **Import / Export** — paste a JSON task export, or copy this task as JSON
* **Duplicate** — create a copy of the task
* **Versions** — view and restore previous versions of the task

## Running a task

If your task subscribes to `mechanic/user/trigger`, `mechanic/user/text`, or `mechanic/user/form`, a **Run task** button appears in the page header.

* **Trigger tasks** run immediately with one click
* **Text tasks** prompt for a text input
* **Form tasks** show a form with fields defined by the task's [user form](../core/tasks/user-form.md) options
