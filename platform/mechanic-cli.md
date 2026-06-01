---
description: Edit Mechanic tasks locally, preview and diff changes, and publish them safely with the Mechanic CLI.
---

# Local task development with the Mechanic CLI

Use the Mechanic CLI when you want to keep a shop's tasks in local files, edit them in your own tools, preview changes with Mechanic, and publish them back when they are ready.

This is the recommended path for task work that belongs in Git, needs review, or benefits from an editor, an AI coding tool, or a repeatable deployment process.

{% hint style="info" %}
The CLI uses Mechanic's task sync API, authenticated with an API token from your shop. If you are building custom scripts, see [API tokens and task sync API](mechanic-task-sync-api.md). For most users, start with the CLI.
{% endhint %}

## How it fits together

Each CLI project is connected to one Mechanic shop. The project contains:

* `mechanic.json`, which identifies the shop and API host
* `.mechanic/links.json`, which remembers which local files match which tasks in Mechanic
* `tasks/*.json`, which are the canonical task files

Mechanic task JSON is the portable format. It is the same general shape used by [Import and export](../core/tasks/import-and-export.md). Because editing long Liquid and Markdown strings inside JSON is awkward, the CLI can also split one task into a helper folder with separate files for Liquid, docs, subscriptions, and JavaScript.

In short:

* JSON files are the source of truth for syncing.
* Helper folders are the comfortable editing format.
* Bundle helper folders back into JSON before publishing.

## Set up a local task project

{% stepper %}
{% step %}

### Install the CLI

```bash
npm install -g @lightward/mechanic-cli
```

{% endstep %}
{% step %}

### Create an API token

In the Mechanic app, open **Settings > API tokens** and create a token for this device, repository, or automation.

Copy the token immediately. Mechanic only shows it once.

{% endstep %}
{% step %}

### Initialize the project

Create a folder for the shop's task files, then initialize it:

```bash
mechanic init --shop example.myshopify.com
```

When prompted, paste the API token from Mechanic. The CLI stores it outside the project folder.

{% endstep %}
{% step %}

### Pull tasks from Mechanic

```bash
mechanic tasks pull
```

This creates local JSON files in `tasks/` and records links to the matching tasks in Mechanic. Running `tasks pull` without a task selector pulls every task for the shop.

If you only want one task later, pass a task selector:

```bash
mechanic tasks pull tasks/order-tagger.json
```

A task selector can be a task JSON file, helper folder, unique local slug, or linked remote task ID. When in doubt, use the full `tasks/<name>.json` path.

{% endstep %}
{% endstepper %}

## Edit, preview, and publish one task

{% stepper %}
{% step %}

### Choose a task

Pick a task file from `tasks/`, or run:

```bash
mechanic tasks list --verbose
```

`--verbose` shows remote task IDs, linked local files, and hashes.

To start from scratch instead, create a local starter task:

```bash
mechanic tasks new order-tagger
```

This creates both `tasks/order-tagger.json` and `tasks/order-tagger/`. Nothing is created in Mechanic until you publish.

{% endstep %}
{% step %}

### Unbundle it for editing

If you pulled an existing task and only have the JSON file, unbundle it before editing Liquid or docs:

```bash
mechanic tasks unbundle tasks/order-tagger.json
```

This creates a helper folder:

```text
tasks/
  order-tagger.json
  order-tagger/
    task.json
    script.liquid
    docs.md
    subscriptions.liquid
```

Tasks created with `tasks new` already have this helper folder.

{% endstep %}
{% step %}

### Edit and bundle

Edit the helper files, then bundle them back into the JSON file:

```bash
mechanic tasks bundle tasks/order-tagger
```

{% endstep %}
{% step %}

### Check status

Check where things stand:

```bash
mechanic tasks status
```

This checks local files and linked tasks in Mechanic, so you can see whether a task is ready, changed locally, unchanged, or conflicted. If a helper folder has changes that have not been bundled, the CLI will stop before publishing and tell you what to run. Use `--local` only when you want to skip the network check.

{% endstep %}
{% step %}

### Preview locally

`tasks preview` sends the local task content to Mechanic's preview engine without saving it. It reports validation errors, previewed actions, and Shopify permissions detected by the previewed paths. Missing Shopify permissions are approved in the Mechanic app after publishing or enabling the task.

```bash
mechanic tasks preview tasks/order-tagger.json
```

Use `--verbose` if you want the terminal to show event, task run, and action run details without switching to JSON output:

```bash
mechanic tasks preview tasks/order-tagger.json --verbose
```

{% endstep %}
{% step %}

### Review the diff

`tasks diff` compares your local task with the current task in Mechanic.

```bash
mechanic tasks diff tasks/order-tagger.json
```

{% endstep %}
{% step %}

### Dry run the publish

`tasks publish --dry-run` validates the selected task, checks for helper-folder drift, checks the current remote task, and prints what would happen without writing to Mechanic.

```bash
mechanic tasks publish tasks/order-tagger.json --dry-run
```

{% endstep %}
{% step %}

### Publish intentionally

`tasks publish` writes the selected local task to Mechanic. Publishing does not enable or disable an existing task. New tasks are created disabled, so you can review and enable them in the app when they are ready.

```bash
mechanic tasks publish tasks/order-tagger.json
```

{% hint style="warning" %}
Use `--force` only when you intentionally want the local file to win over changes made in Mechanic or another checkout. The default workflow is designed to stop before stale local files overwrite newer remote changes.
{% endhint %}

{% endstep %}
{% endstepper %}

## Before publishing

For a normal task change, this is the confidence loop:

```bash
mechanic tasks status
mechanic shop status
mechanic tasks preview tasks/order-tagger.json
mechanic tasks diff tasks/order-tagger.json
mechanic tasks publish tasks/order-tagger.json --dry-run
```

`shop status` shows whether the shop has running or waiting runs. It is a useful check before publishing changes to a busy shop.

Use `tasks open` to jump from a local task file, helper folder, slug, or remote task ID to the task in Mechanic:

```bash
mechanic tasks open tasks/order-tagger.json
```

## Git and GitHub Actions

You can commit `mechanic.json`, `.mechanic/links.json`, and `tasks/` to Git. This gives your task changes history, review, and rollback.

Once the local workflow is working, the CLI can generate optional GitHub Actions:

```bash
mechanic github init
```

The generated workflows can validate task files on pull requests, run a manual dry-run/deploy, and open a sync-back PR for changes pulled from Mechanic. After a deploy, the workflow also opens or updates a sync-state PR so `.mechanic/links.json` and task hashes stay current.

{% hint style="warning" %}
Only add deploy or sync workflows to repositories maintained by people you trust. Those workflows use `MECHANIC_API_TOKEN`, which can read, preview, and publish tasks for the shop.
{% endhint %}

The sync-from-app workflow is update-only in this version. It does not delete local files for tasks deleted in Mechanic.

## AI and agent workflows

The CLI pairs well with AI coding tools:

* Use [Mechanic MCP Server](mcp.md) so your assistant can search Mechanic docs and task library examples.
* Use [Mechanic Agent Skills](agent-skills.md) so your assistant knows how to write Mechanic tasks.
* Use the CLI to preview, diff, dry-run, and publish the task files the assistant edits.

For agent-friendly output, many commands support machine-readable or automation-friendly flows, such as:

```bash
mechanic tasks preview tasks/order-tagger.json --json
```

## Related

* [API tokens and task sync API](mechanic-task-sync-api.md)
* [Import and export](../core/tasks/import-and-export.md)
* [Previews](../core/tasks/previews/)
* [Mechanic MCP Server](mcp.md)
* [Mechanic Agent Skills](agent-skills.md)
