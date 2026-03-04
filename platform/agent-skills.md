# 🤖 Mechanic Agent Skills

Install Mechanic's task-writing expertise directly into your AI coding tool. The `mechanic-task-writer` skill teaches your AI assistant how to write production-ready Mechanic tasks with correct subscriptions, preview mode, GraphQL queries, bulk operations, and complete importable JSON output.

***

## What are Agent Skills?

Agent Skills are markdown instruction files that get installed into your AI coding tool's configuration directory. When your AI assistant starts a conversation, it reads these files automatically — giving it specialized knowledge without you having to explain anything or paste instructions.

Think of it like a reference manual that your AI always has open. The `mechanic-task-writer` skill contains Mechanic's task-writing patterns, conventions, and common pitfalls, so your assistant knows how to produce correct task code from the start.

***

## What the skill provides

When installed, the `mechanic-task-writer` skill gives your assistant deep knowledge of Mechanic's patterns:

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

### Step 1: Run the install command

Open your terminal and run:

```bash
npx skills add lightward/mechanic-skills --skill mechanic-task-writer
```

The CLI is interactive — it will walk you through a few prompts:

1. **Select agents.** The CLI shows a list of AI tools. Tools in the "Universal" group (Cursor, Codex, Gemini CLI, etc.) are always included. If your tool is under "Additional agents" (like Claude Code), use the arrow keys to find it and press **space** to select it, then **enter** to confirm.
2. **Choose scope.** Select **Project** (installs to your current directory) or **Global** (available in all projects).
3. **Choose install method.** Select **Symlink** (recommended — single source, easy updates) or **Copy** (independent copy).

The CLI will then place the skill file in the correct directory for each selected tool.

{% hint style="info" %}
You can also install all skills from the package by omitting the `--skill` flag:

```bash
npx skills add lightward/mechanic-skills
```

{% endhint %}

### Step 2: Verify the install

{% tabs %}
{% tab title="Claude Code" %}

In Claude Code, type `/skills` to see your installed skills. You should see `mechanic-task-writer` listed.

{% hint style="info" %}
The `/skills` command requires the native Claude Code build. If you installed Claude Code via npm, run `claude install` first to switch to the native version.
{% endhint %}

You can also check the file directly:

```bash
ls ~/.claude/skills/
```

If you installed at the project level, check `.claude/skills/` in your project directory instead.

{% endtab %}

{% tab title="Cursor" %}

Check that the skill file exists:

```bash
ls ~/.cursor/skills/
```

You should see a `mechanic-task-writer` file or directory. If you installed at the project level, check `.agents/skills/` in your project directory instead.

{% endtab %}

{% tab title="Codex CLI" %}

In Codex, type `$skill` to see available skills. You should see `$mechanic-task-writer` listed.

You can also check the file directly:

```bash
ls ~/.codex/skills/
```

{% endtab %}

{% tab title="Gemini CLI" %}

Check that the skill file exists:

```bash
ls ~/.gemini/skills/
```

The skill may also be in `~/.agents/skills/`.

{% endtab %}

{% tab title="Manual install" %}

If the CLI doesn't detect your tool, you can copy the skill file manually from the [GitHub repository](https://github.com/lightward/mechanic-skills). Download the `SKILL.md` file and place it in your tool's skills directory.

{% endtab %}
{% endtabs %}

### Step 3: Restart your tool

Close and reopen your AI coding tool. Skills are loaded when a session starts, so you need to restart for the new skill to take effect.

***

## Using the skill

Once installed, the skill works automatically. You can use it in two ways:

* **Just ask naturally.** Say something like "help me build a Mechanic task that auto-tags orders over $100" and your assistant will use the skill's patterns automatically.
* **Use a slash command.** In Claude Code, type `/mechanic-task-writer` to explicitly invoke the skill. In Codex, use `$mechanic-task-writer`.

Your assistant will produce complete, importable task JSON with correct subscriptions, preview blocks, and GraphQL queries.

***

## Troubleshooting

**The command ran but nothing happened.** Check that Node.js 18+ is installed (`node --version`). The `npx skills add` command needs Node to run.

**I don't see my tool in the agent list.** Look under "Additional agents" — not all tools are in the universal group. Use the arrow keys to scroll down, then press **space** to select your tool.

**The skill file isn't in the expected directory.** The CLI places files based on which agents you selected during the interactive prompts. If it didn't detect your tool, use the manual install tab above.

**Claude Code says `/skills` is not a command.** You're on the npm version of Claude Code. Run `claude install` to switch to the native build, which supports `/skills`.

**My AI assistant doesn't seem to know Mechanic patterns.** Make sure you restarted your tool after installing. Skills are loaded at session start. Also verify the file exists in the correct directory using the verification step above.

**I want to update the skill.** Run the same install command again — it will overwrite the existing file with the latest version.

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
