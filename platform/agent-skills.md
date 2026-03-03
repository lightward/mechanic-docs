# 🤖 Mechanic Agent Skills

Install Mechanic's task-writing expertise directly into your AI coding tool. The `mechanic-task-writer` skill teaches your AI assistant how to write production-ready Mechanic tasks with correct subscriptions, preview mode, GraphQL queries, bulk operations, and complete importable JSON output.

***

## How it works

Agent Skills are reusable knowledge packages that run inside your AI assistant's context. When installed, the `mechanic-task-writer` skill gives your assistant deep knowledge of Mechanic's patterns without needing to look anything up:

* Complete importable JSON task output — not just a Liquid script
* Correct async/sync patterns (read with `| shopify`, write with `{% action %}`)
* Preview blocks for every subscribed event topic
* GraphQL-first Shopify API usage
* Proper webhook ID handling (`admin_graphql_api_id`)
* Bulk operation patterns with `__typename` and `__parentId`
* Two-pass workflows via `mechanic/actions/perform`
* Case-insensitive tag comparison
* Task options with correct suffixes

***

## Requirements

* **Node.js 18 or higher** installed on your system.
* An **AI coding tool** that supports Agent Skills, such as Claude Code, Cursor, Codex CLI, or Gemini CLI.

***

## Install the skill

Run this command in your terminal:

```bash
npx skills add lightward/mechanic-skills --skill mechanic-task-writer
```

This installs the skill globally for your AI tool. You can also install all skills from the package by omitting the `--skill` flag:

```bash
npx skills add lightward/mechanic-skills
```

### Per-tool instructions

{% tabs %}
{% tab title="Claude Code" %}

The skill installs to `~/.claude/skills/`. After running the install command, restart Claude Code to load the skill.

{% endtab %}

{% tab title="Cursor" %}

The skill installs to `.cursor/skills/` (project-level) or `~/.cursor/skills/` (global). After running the install command, restart Cursor to load the skill.

{% endtab %}

{% tab title="Codex CLI" %}

The skill installs to `~/.codex/skills/`. After running the install command, restart Codex to load the skill.

{% endtab %}

{% tab title="Gemini CLI" %}

The skill installs to `~/.gemini/skills/` or `~/.agents/skills/`. After running the install command, restart Gemini CLI to load the skill.

{% endtab %}

{% tab title="Manual" %}

If you prefer not to use the CLI, copy the skill folder from the [GitHub repository](https://github.com/lightward/mechanic-skills) directly into your AI tool's skills directory.

{% endtab %}
{% endtabs %}

***

## Using with the MCP server

For the best results, use Agent Skills together with the [Mechanic MCP Server](mcp.md). The MCP server gives your AI assistant access to the task library and documentation at query time, while Agent Skills provide the patterns and conventions for writing correct task code. Together, they cover both reference and execution.

***

## Related resources

* [Mechanic MCP Server](mcp.md) — connect your AI to the task library and docs
* [Using AI with Mechanic](../ai.md) — overview of all Mechanic AI tools
* [Task library](../resources/task-library/) — browse 350+ pre-built automation tasks
* [GitHub: mechanic-skills](https://github.com/lightward/mechanic-skills) — source repository
* [skills.sh](https://skills.sh) — Agent Skills directory
