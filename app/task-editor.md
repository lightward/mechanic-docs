---
description: Create, edit, and test Mechanic tasks with the built-in code editor.
---

# Task editor

The task editor is where you write and configure Mechanic [tasks](../core/tasks/). It has two modes: **Basic** for simple configuration, and **Advanced** for full access to code, preview events, JavaScript, runtime settings, permissions, and documentation.

Switch between modes using the **Advanced/Basic mode** toggle in the page actions menu. You can set your default mode in [Settings](settings.md).

## Basic mode

Basic mode shows a simplified view with two sections:

* **Options** — configure the task's input parameters (the fields defined by the task's [options](../core/tasks/options/))
* **Documentation** — view the task's documentation, if any

This is the default mode for merchants who are using pre-built tasks and don't need to edit code.

## Advanced mode

Advanced mode provides a tabbed editor with a resizable sidebar.

### Code tab

The main editing surface, with two editors:

* **Subscriptions** — define which [event topics](../core/events/topics.md) trigger this task, one per line
* **Code** — write the task's [Liquid script](../platform/liquid/)

The code editor includes syntax highlighting, autocomplete, and a minimap for longer scripts. Press `Cmd/Ctrl + S` to save.

### Preview events tab

Create test events to see how your task will behave. Each preview event has:

* **Description** — a label for the test case
* **Topic** — the event type being tested
* **Data** — the event payload as JSON

The task [preview](../core/tasks/previews/) updates automatically as you edit, showing the actions your task will generate, any errors, and any Shopify permission changes.

### JavaScript tab

Add JavaScript that runs in your customers' browser (on the online storefront or order status page). Supports Liquid templating within the JavaScript.

### Runtime tab

Configure execution behavior:

* **Shopify Admin API version** — which [API version](../core/shopify/api-versions.md) the task uses
* **Perform action runs in sequence** — execute actions one at a time instead of in parallel
* **Halt the sequence when one fails** — stop processing remaining actions if one fails

### Permissions tab

A read-only view of the Shopify API [permissions](../core/tasks/permissions.md) your task requires, automatically detected from your code and preview.

### Docs & Tags tab

* **Tags** — organize tasks with custom labels
* **Documentation** — write Markdown notes or instructions for the task, with a live preview

## Sidebar

In advanced mode, the right sidebar shows:

* **Options** — configure task input parameters with live validation
* **Preview** — real-time preview of the actions your task will generate

The sidebar is resizable and collapsible.

## Page actions

The actions menu (accessible from the page header) includes:

* **Events** — view all events processed by this task
* **Import** — paste a JSON task export to merge into the current task
* **Export** — copy the task as JSON
* **Duplicate** — create a copy of the task (starts disabled)
* **Versions** — view and restore previous versions of the task

## Running a task

If your task subscribes to `mechanic/user/trigger`, `mechanic/user/text`, or `mechanic/user/form`, a **Run task** button appears in the page header.

* **Trigger tasks** run immediately with one click
* **Text tasks** prompt for a text input
* **Form tasks** show a form with fields defined by the task's [user form](../core/tasks/user-form.md) options
