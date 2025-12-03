# Mechanic MCP Server

Connect your AI assistant to Mechanic's task library and documentation. The Mechanic Model Context Protocol (MCP) server enables your AI assistant to search tasks, explore documentation, and fetch task code.

***

## How it works

Your AI assistant uses the MCP server to read and interact with Mechanic's resources:

1. Ask your AI assistant to help with Mechanic task development or automation questions.
2. The assistant searches Mechanic's task library and documentation based on your prompt.
3. The MCP server provides access to 350+ pre-built tasks and comprehensive Mechanic documentation, so your assistant can provide accurate code, solutions, and guidance based on current best practices.

***

## Requirements

Before you set up the Mechanic MCP server, make sure you have:

* **Node.js 18 or higher** installed on your system.
* An **AI development tool** that supports MCP, such as Claude Desktop, Claude Code, Cursor, Codex CLI, or Gemini CLI.

***

## What you can ask your AI assistant

After you set up the MCP server, you can ask your AI assistant questions like:

* "Find tasks that auto-tag orders"
* "How do I subscribe to Shopify product creation events?"
* "Show me the code for the 'Auto-tag customers by order tier' task"
* "What tasks use the bulk operations API?"
* "How do I use the cache action in Mechanic?"
* "Find similar tasks to 'email customer when order is fulfilled'"
* "Write a task that sends an email when inventory gets low"

Your AI assistant will use the MCP server to search Mechanic's task library and documentation when providing responses.

***

## Available resources

The MCP server provides access to:

* **350+ pre-built automation tasks** from [tasks.mechanic.dev](https://tasks.mechanic.dev)
* **Complete Mechanic documentation** from [learn.mechanic.dev](https://learn.mechanic.dev)
* Task subscriptions, options, and full Liquid scripts
* Documentation on Liquid templating, actions, events, and integrations

***

## Set up the server

The server runs locally in your development environment and doesn't require authentication.

### Step 1: Configure your AI development tool

Add configuration code that tells your AI tool how to connect to and use the Mechanic MCP server. This configuration enables your AI assistant to automatically access Mechanic's task library and documentation when you ask questions.

#### Claude Desktop and Claude Code

**For Claude Desktop:**

1. Open the app and access your configuration file through settings
2. Add the JSON configuration below to your MCP servers section
3. Save and restart Claude Desktop

**For Claude Code:**

1. Run this command in your terminal:

   ```bash
   claude mcp add --scope user --transport stdio mechanic-mcp -- npx -y @lightward/mechanic-mcp@latest
   ```

2. Restart Claude Code to load the server

**Manual JSON configuration (both):**

{% tabs %}
{% tab title="Configuration" %}

```json
{
  "mcpServers": {
    "mechanic-mcp": {
      "command": "npx",
      "args": ["-y", "@lightward/mechanic-mcp@latest"]
    }
  }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
For more information, read the [Claude Desktop MCP guide](https://modelcontextprotocol.io/quickstart/user).
{% endhint %}

#### Cursor

1. Open Cursor and go to **Cursor** > **Settings** > **Cursor Settings** > **Tools and integrations** > **New MCP server**.

2. Add this configuration to your MCP servers:

{% tabs %}
{% tab title="Configuration" %}

```json
{
  "mcpServers": {
    "mechanic-mcp": {
      "command": "npx",
      "args": ["-y", "@lightward/mechanic-mcp@latest"]
    }
  }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
For more information, see the [Cursor MCP documentation](https://docs.cursor.com/context/model-context-protocol).
{% endhint %}

1. Save your configuration and restart Cursor.

#### Codex CLI

1. Add this configuration to your `~/.codex/config.toml` file:

{% tabs %}
{% tab title="Configuration" %}

```toml
[mcp_servers.mechanic-mcp]
command = "npx"
args = ["-y", "@lightward/mechanic-mcp@latest"]
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
Codex uses TOML format with `mcp_servers` (snake_case) instead of JSON with `mcpServers` (camelCase).
{% endhint %}

1. Restart Codex to load the new MCP server configuration.

#### Gemini CLI

1. Add this configuration using the Gemini CLI:

{% tabs %}
{% tab title="Configuration" %}

```json
{
  "mcpServers": {
    "mechanic-mcp": {
      "command": "npx",
      "args": ["-y", "@lightward/mechanic-mcp@latest"]
    }
  }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
By default, this adds the server to your project configuration. To make it available across all projects, add the `--scope user` flag. For more information, see the [Gemini CLI MCP documentation](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html).
{% endhint %}

1. Restart Gemini CLI to load the new MCP server configuration.

***

## Available tools

The Mechanic MCP server provides the following tools:

### `search_tasks`

Search across Mechanic's task library to find tasks matching your query. Returns task titles, descriptions, tags, subscriptions, and public URLs.

Best for discovering existing automation solutions before building custom tasks. Results include subscription event topics, task options, and tags for filtering.

**Parameters:**

* `query` (required): Search terms to match against task titles, descriptions, and content
* `limit` (optional): Maximum number of results to return (default: 10, max: 50)
* `offset` (optional): Number of results to skip for pagination (default: 0)
* `tags` (optional): Filter by task tags
* `subscriptions` (optional): Filter by event subscription topics
* `fuzzy` (optional): Enable fuzzy matching for typo tolerance
* `fuzzyMaxEdits` (optional): Maximum edit distance for fuzzy matching (max: 2)

***

### `search_docs`

Search across all Mechanic documentation to find relevant pages matching your query. Returns documentation titles, paths, and public URLs.

Best for learning about Mechanic concepts, Liquid templating, actions, and platform features. Returns quick results from across the entire documentation site.

**Parameters:**

* `query` (required): Search terms to match against documentation content
* `limit` (optional): Maximum number of results to return (default: 10, max: 50)
* `offset` (optional): Number of results to skip for pagination (default: 0)
* `fuzzy` (optional): Enable fuzzy matching for typo tolerance

***

### `get_task`

Retrieve complete task details including subscriptions, Liquid script, options, and JavaScript blocks. Use this to see the full implementation of a specific task.

**Parameters:**

* `id` (required): Task handle or ID (e.g., "auto-tag-customers-by-sales-channel" or "task:auto-tag-customers-by-sales-channel")

**Returns:**

* Task metadata (name, tags, URL)
* Event subscriptions and subscription template
* Complete Liquid script
* Task options with defaults
* Online store JavaScript (if present)
* Order status JavaScript (if present)

{% hint style="info" %}
When sharing task code, return only the relevant parts (subscriptions and script), not the full JSON export. Always include the public task URL.
{% endhint %}

***

### `get_doc`

Retrieve the full content of a documentation page. Provides complete documentation context without chunking.

**Parameters:**

* `id` (required): Documentation ID from search results (e.g., "doc:core/tasks/code/action-objects.md")

**Returns:**

* Full markdown content
* Documentation title and path
* Public documentation URL

{% hint style="info" %}
Documentation pages are also available as MCP resources through the `mechanic-docs://` URI scheme.
{% endhint %}

***

### `similar_tasks`

Find tasks related to a specific task by analyzing shared tags, event subscriptions, and title similarity. Useful for discovering alternative approaches or complementary automations.

**Parameters:**

* `handle` (required): Task handle or ID to find similar tasks for
* `limit` (optional): Maximum number of similar tasks to return (default: 10)

***

### `refresh_index`

Refresh and rebuild the search index from bundled data. Generally not needed as the server includes pre-built indexes.

***

## Usage best practices

When working with your AI assistant and the Mechanic MCP server:

* **Discover before building**: Search for existing tasks before creating custom solutions. The library contains 350+ battle-tested tasks.
* **Use GraphQL**: Prefer Shopify's GraphQL Admin API in task code. REST is deprecated in Mechanic.
* **Reference public URLs**: Always cite tasks using their public URLs (tasks.mechanic.dev) and docs using learn.mechanic.dev URLs.
* **Get full task details**: Use `get_task` to see complete implementations, including subscriptions, scripts, and options.
* **Find related work**: Use `similar_tasks` to discover alternative approaches or complementary automations.

***

## Related resources

* [Task library](../resources/task-library/) - Browse and contribute to Mechanic's task library
* [Writing tasks](../core/tasks/) - Learn how to write Mechanic tasks
* [Liquid templating](liquid/) - Mechanic's Liquid implementation
* [Actions](../core/actions/) - Available action types for tasks
* [Events and subscriptions](../core/events/) - Understanding Mechanic's event system
