---
description: Use Mechanic API tokens with the task sync API for trusted scripts, agents, CI, and the Mechanic CLI.
---

# API tokens and task sync API

Mechanic API tokens authenticate task sync workflows for one shop. They are used by the [Mechanic CLI](mechanic-cli.md), and may also be used by trusted scripts, agents, and CI jobs that need direct HTTP access to the same task sync surface.

{% hint style="info" %}
The CLI is the recommended client. It wraps this API with local task validation, content hashes, idempotency keys, preview commands, diff output, and safer publish flows. Direct HTTP use is for CLI-compatible automation, not a broad public API platform.
{% endhint %}

## API tokens

Create API tokens in the Mechanic app at **Settings > API tokens**.

Tokens are:

* shop-scoped
* shown once at creation
* revocable from Mechanic settings
* visible later only by name, creator, creation time, last-used time, and status
* accepted as `Authorization: Bearer <token>`
* usable as `MECHANIC_API_TOKEN` for the CLI and CI jobs

Treat tokens like passwords. A token can read task sync payloads and can preview and publish tasks for its shop until revoked.

Use one token per device, repository, or automation. Each shop can have up to five active API tokens. If a token is leaked, revoke it and create a replacement.

{% hint style="warning" %}
API tokens are not granularly scoped in this version. Do not share a token with untrusted repositories, third-party scripts, or agents you would not trust to read and publish tasks for the shop.
{% endhint %}

## Base URL and authentication

Production API base URL:

```text
https://api.mechanic.dev
```

Authenticate with a bearer token:

```bash
curl https://api.mechanic.dev/v1/auth/verify \
  -H "Authorization: Bearer $MECHANIC_API_TOKEN"
```

Without an expected shop, this only confirms that the token is valid. To verify that a token is valid for the shop your automation intends to use, include the shop domain:

```bash
curl https://api.mechanic.dev/v1/auth/verify \
  -H "Authorization: Bearer $MECHANIC_API_TOKEN" \
  -H "X-Mechanic-Shop-Domain: example.myshopify.com"
```

For local CLI use, store the token outside the project with:

```bash
mechanic auth login
```

For CI jobs, store the token as a secret named `MECHANIC_API_TOKEN` and expose it to the job environment. Avoid typing raw tokens into shared shell commands, where history or process listings may expose them.

## Endpoints

This API is intentionally narrow. It covers task sync, task preview, shop queue
status, and Shopify API deprecation visibility for the authenticated shop. Use
direct HTTP only for trusted automation that can safely store a shop API token.

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/v1/auth/verify` | Verify the API token; include an expected shop to receive shop and token context |
| `GET` | `/v1/shop/status` | Read current queue, backlog, and lag status |
| `GET` | `/v1/shop/deprecations` | Read unresolved Shopify API deprecations reported by the shop's tasks |
| `GET` | `/v1/tasks` | List tasks available to sync |
| `GET` | `/v1/tasks/:id` | Read one task for sync |
| `POST` | `/v1/tasks/preview` | Preview local task content without saving |
| `POST` | `/v1/tasks/:id/preview` | Preview local draft content against an existing task, or preview the current saved task, without saving |
| `POST` | `/v1/tasks` | Create a task from sync payload |
| `PUT` | `/v1/tasks/:id` | Update a task from sync payload |

{% hint style="info" %}
There is no `tasks new` API endpoint. `mechanic tasks new` is a local CLI command that creates a starter task file and helper folder in an initialized project. When that new local task is published, the CLI uses `POST /v1/tasks`, and Mechanic creates the remote task disabled.

`POST /v1/tasks` always creates a new task. It does not update an existing task by matching the task name or local slug. For direct HTTP creates, send an `Idempotency-Key` header with a UUID generated for the task you intend to create. Mechanic uses that UUID as the created task ID. Retrying with the same UUID returns the existing task instead of creating a duplicate, so use a different UUID for each intended task.
{% endhint %}

For most local task work, prefer the CLI commands that wrap these endpoints:

```bash
mechanic tasks preview order-tagger --json
mechanic tasks publish order-tagger --dry-run
mechanic tasks publish order-tagger
mechanic shop status
mechanic shop deprecations
```

Use `mechanic shop status --json`, or `GET /v1/shop/status` directly, when you
want an external monitor to alert on queue lag or waiting runs for the shop.

Use `mechanic shop deprecations --json` when a trusted script, agent, or
dashboard needs structured deprecation data. Use `GET /v1/shop/deprecations`
directly only when the automation needs to make HTTP requests itself. Add
`task_id` to focus on one task:

```bash
curl "https://api.mechanic.dev/v1/shop/deprecations?task_id=<task-id>" \
  -H "Authorization: Bearer $MECHANIC_API_TOKEN"
```

The response is capped and includes `total_count`, `limit`, and `truncated`.
Deprecation entries include task identity, task Shopify API version, request API
version, request method/path, occurrence count, and first/last occurrence times.
They do not include request bodies, request headers, response headers, tokens, or
other raw payload data.

`GET /v1/tasks` and `GET /v1/tasks/:id` also include `shopify_api_version` in
task sync envelopes, so scripts can see the configured API version without
fetching or parsing the task JSON separately.

## Preview and publish behavior

Preview requests do not create, update, save, enable, or run a task. They ask Mechanic to evaluate local or remote task content through preview mode and return the result.

Use `POST /v1/tasks/preview` for local task content that is not linked to an existing Mechanic task. Use `POST /v1/tasks/:id/preview` when you want to preview draft local content in the context of an existing task. If no task body is sent to `/v1/tasks/:id/preview`, Mechanic previews the current saved task.

Publishing writes task content to Mechanic. It does not enable or disable an existing task. New tasks created through task sync are created disabled. Review and enable them in the app when they are ready.

Updates require remote-change protection. Send `previous_content_hash` with `PUT /v1/tasks/:id` so Mechanic can reject stale updates instead of overwriting newer changes. This must be the `content_hash` from the task version your client last read from Mechanic, not a hash of the outgoing local payload. Use `force` only as an intentional bypass when your local payload should replace the current Mechanic task.

## Rate limits and errors

The task sync API has its own rate limits, separate from Shopify Admin API rate limits. If Mechanic returns `429 Too Many Requests`, respect the `Retry-After` response and retry later.

Current task sync limits:

* public API requests: 120/min per IP, checked before token authentication
* authenticated API token requests: 300/min per token
* task writes: 120/min per token
* shop status requests: 60/min per token
* task previews: 30/min per token
* preview concurrency: at most 2 running previews per token, and at most 4 running previews per shop through the task sync API

Avoid tight preview loops in CI or agents.

Common responses:

| Status | Meaning |
| --- | --- |
| `401` | Missing, invalid, or revoked API token |
| `403` | Token is not allowed to access this shop or API surface |
| `400` | Invalid request, validation error, or missing update guard such as `previous_content_hash` or `force` |
| `404` | Task not found for this shop |
| `409` | Remote task changed since the last known content hash |
| `429` | Rate limit reached; retry after the indicated delay |

## GitHub Actions

If you use generated GitHub Actions, store the token as a repository or organization secret named `MECHANIC_API_TOKEN`.

Pull request validation does not need a Mechanic token. Deploy and sync workflows do.

{% hint style="warning" %}
Use deploy and sync workflows only in trusted repositories. Consider GitHub environment protection, branch restrictions, and reviewer approval before workflows can publish to Mechanic.
{% endhint %}

## What is not included yet

This is not a general-purpose Mechanic API reference. This version does not document SDKs, token scopes, task deletion, task enabling/disabling, event APIs, run APIs, webhook management APIs, fleet management, or formal public API versioning beyond the task sync surface.

Start with [Local task development with the Mechanic CLI](mechanic-cli.md) unless you specifically need direct HTTP automation.
