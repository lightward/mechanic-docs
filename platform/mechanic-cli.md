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

The Mechanic CLI requires Node.js 22 or higher.

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
mkdir example-mechanic-tasks
cd example-mechanic-tasks
mechanic init --shop example.myshopify.com
```

When prompted, paste the API token from Mechanic. The CLI stores it outside the project folder.

{% endstep %}
{% step %}

### Choose how to start

After initializing the project, choose one path.

**Bring one existing task into local files:**

```bash
mechanic tasks list --verbose
mechanic tasks pull <remote-task-id>
```

This creates one local JSON file in `tasks/` and records the link to the matching task in Mechanic. Use this when you want to start with a specific task.
The task list also shows each task's configured Shopify API version, which is
useful when deciding which tasks may need an API version review.

**Bootstrap the repo with every task in the shop:**

```bash
mechanic tasks pull
```

Running `tasks pull` without a task selector pulls every task for the shop. Use this when you are intentionally setting up a Git repo for all of a shop's tasks.

**Start from a new local task:**

```bash
mechanic tasks new order-tagger
```

This creates a starter task in `tasks/order-tagger.json` and `tasks/order-tagger/`. Nothing is created in Mechanic until you publish.

For most commands, the simplest selector is the local task slug, like `order-tagger`. The CLI also accepts a task JSON file, helper folder, or linked remote task ID. If a slug matches more than one local file, the CLI will ask for the full `tasks/<name>.json` path.

Task names and local file names are related, but they are not the same thing. When the CLI first pulls or creates a task, it uses the task name to choose a readable local slug like `order-tagger`, which becomes `tasks/order-tagger.json`.

If you rename the task in Mechanic, the next pull keeps the existing local file name and updates the `name` field inside the JSON. If you rename the local JSON file or helper folder by hand, the task may appear unlinked. Prefer keeping local filenames stable, and run `tasks status` after renaming anything.

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

`--verbose` shows remote task IDs, linked local files, configured Shopify API
versions, and sync details.

To start from scratch instead, create a new blank local task:

```bash
mechanic tasks new order-tagger
```

This creates a blank starter task in both `tasks/order-tagger.json` and `tasks/order-tagger/`. You can use it any time in an initialized CLI project; it does not require a fresh repository. Nothing is created in Mechanic until you publish, and the command will not overwrite existing local task files unless you pass `--force`.

{% endstep %}
{% step %}

### Unbundle it for editing

If you pulled an existing task and only have the JSON file, unbundle it before editing Liquid or docs:

```bash
mechanic tasks unbundle order-tagger
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

Tasks created with `tasks new` already have this helper folder, so you can edit them without running `tasks unbundle` first.

{% endstep %}
{% step %}

### Edit and bundle

Edit the helper files, then bundle them back into the JSON file:

```bash
mechanic tasks bundle order-tagger
```

{% endstep %}
{% step %}

### Check status

Check where things stand:

```bash
mechanic tasks status
```

This checks local files and task links, so you can see whether a task is ready or needs bundling. For smaller projects, it also checks the current linked tasks in Mechanic. For larger projects, repo-wide status stays local to avoid hundreds of API calls; pass a task selector when you want to check one task's remote state:

```bash
mechanic tasks status order-tagger
```

If a helper folder has changes that have not been bundled, the CLI will stop before publishing and tell you what to run. Use `--local` when you explicitly want to skip remote checks.

{% endstep %}
{% step %}

### Preview locally

`tasks preview` sends the local task content to Mechanic's preview engine without saving it. It reports validation errors, previewed actions, and Shopify permissions detected by the previewed paths. Missing Shopify permissions are approved in the Mechanic app after publishing or enabling the task.

```bash
mechanic tasks preview order-tagger
```

Use `--verbose` if you want the terminal to show event, task run, and action run details without switching to JSON output:

```bash
mechanic tasks preview order-tagger --verbose
```

{% endstep %}
{% step %}

### Review the diff

`tasks diff` compares the version currently saved in Mechanic with your local
file. In diff output, `-` is Mechanic and `+` is your local file.

If only your local file changed, continue to the publish preflight. If Mechanic
changed and your local file has not, pull normally to bring the Mechanic version
into your repo. If both changed, reconcile the changes first when you can.

When you intentionally need one version to replace the other, the command tells
the CLI which version to keep:

```bash
mechanic tasks pull <remote-task-id> --force
mechanic tasks publish order-tagger --force
```

`pull --force` keeps the current Mechanic version. `publish --force` keeps your
local file.

```bash
mechanic tasks diff order-tagger
```

{% endstep %}
{% step %}

### Check the publish plan

`tasks publish --dry-run` is a publish preflight. It checks whether publishing would be safe, including helper-folder drift and remote conflicts, then prints what would create, update, stay unchanged, or stop before writing anything to Mechanic.

```bash
mechanic tasks publish order-tagger --dry-run
```

{% endstep %}
{% step %}

### Publish intentionally

`tasks publish` writes the selected local task to Mechanic. If the task is already enabled, the newly published version is live for future runs immediately. Publishing does not turn enabled tasks off or disabled tasks on.

New tasks are created disabled, so you can review and enable them in the app when they are ready.

```bash
mechanic tasks publish order-tagger
```

After publishing, open the task in Mechanic to review the saved version:

```bash
mechanic tasks open order-tagger
```

In Mechanic, confirm the saved version, approve any Shopify permissions if prompted, and enable the task if it was newly created. Existing enabled tasks keep running with the published version.

{% hint style="warning" %}
Use `publish --force` only when you intentionally want the local file to replace changes made in Mechanic or another checkout. The default workflow is designed to stop before stale local files overwrite newer remote changes.
{% endhint %}

{% endstep %}
{% endstepper %}

## Before publishing

For a normal task change, this is the confidence loop:

```bash
mechanic tasks status order-tagger
mechanic tasks preview order-tagger
mechanic tasks diff order-tagger
mechanic tasks publish order-tagger --dry-run
```

Use `tasks open` to jump from a local task file, helper folder, slug, or remote task ID to the task in Mechanic:

```bash
mechanic tasks open order-tagger
```

## Monitor queue lag

`shop status` shows the shop's current Mechanic queue and backlog: running
runs, waiting runs, queue lag, and the tasks, actions, or event topics that are
contributing to the wait.

Use it when you want to monitor whether a shop is falling behind:

```bash
mechanic shop status
```

For external monitoring tools, use JSON output from a scheduled job and alert on
waiting runs or queue lag:

```bash
mechanic shop status --json
```

This can be used with a service like Cronitor, UptimeRobot, Better Stack, or
your own monitoring system. It is not required for publishing task changes.
For integrations that prefer direct HTTP requests, use
[`GET /v1/shop/status`](mechanic-task-sync-api.md) with a Mechanic API token.

## Review Shopify API deprecations

`shop deprecations` shows unresolved Shopify API deprecations reported by tasks
in the configured shop:

```bash
mechanic shop deprecations
```

Use it when you want to find tasks that are still calling deprecated Shopify API
surfaces. The output includes the task's configured Shopify API version, the API
version seen in the deprecated request, the request path, and the last time
Mechanic saw it.

To focus on one linked task, pass the same task selector you would use for
preview, diff, or publish:

```bash
mechanic shop deprecations order-tagger
```

Use `--json` when a script, agent, or dashboard needs structured output.

## Git and GitHub Actions

You can commit `mechanic.json`, `.mechanic/links.json`, and `tasks/` to Git. This gives your task changes history, review, and rollback.

Once the local workflow is working, the CLI can generate optional GitHub Actions:

```bash
mechanic github init
```

Local CLI usage works without GitHub Actions. If you add them, the generated
workflows can validate task files on pull requests, run a manual dry-run/deploy,
and open a sync-back PR for changes pulled from Mechanic. After a deploy, the
workflow also opens or updates a sync-state PR so `.mechanic/links.json` and
task hashes stay current.

`mechanic github init` creates:

```text
.github/workflows/mechanic-validate.yml
.github/workflows/mechanic-deploy.yml
.github/workflows/mechanic-sync-from-app.yml
```

Pull request validation does not need a Mechanic token. Deploy and sync
workflows do. Store that token as a GitHub secret named `MECHANIC_API_TOKEN`.

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
mechanic tasks preview order-tagger --json
```

## Related

* [API tokens and task sync API](mechanic-task-sync-api.md)
* [Import and export](../core/tasks/import-and-export.md)
* [Previews](../core/tasks/previews/)
* [Mechanic MCP Server](mcp.md)
* [Mechanic Agent Skills](agent-skills.md)
