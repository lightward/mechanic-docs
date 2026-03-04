# 🛍️ Shopify Dev MCP Server

The Shopify Dev MCP server is built and maintained by Shopify. It gives your AI coding tool direct access to Shopify's API documentation, GraphQL schema introspection, and query validation — making it a useful companion to the [Mechanic MCP Server](mcp.md) when writing tasks.

{% hint style="info" %}
This is a Shopify tool, not a Mechanic tool. For Mechanic-specific task library access and documentation, use the [Mechanic MCP Server](mcp.md).
{% endhint %}

***

## Why it's useful for Mechanic development

Mechanic tasks interact with Shopify's APIs via GraphQL. The Shopify Dev MCP server helps your AI assistant:

* **Introspect the GraphQL schema** — look up types, fields, and input objects directly instead of guessing
* **Validate GraphQL queries** — catch syntax errors and invalid fields before you run a task
* **Search Shopify documentation** — find current API patterns, required permissions, and usage examples across all nine supported APIs (Admin GraphQL, Storefront, Functions, and more)
* **Stay current** — pulls from Shopify's live documentation, so it reflects the latest API version

When used alongside the Mechanic MCP server, your AI assistant has access to both Mechanic's task patterns and Shopify's full API surface — which is what you need for writing tasks that work.

***

## Requirements

* **Node.js 18 or higher** installed on your system.
* An **AI coding tool** that supports MCP, such as Claude Code, Claude Desktop, Cursor, Codex CLI, or Gemini CLI.

***

## Set up the server

The server runs locally and doesn't require authentication.

{% tabs %}
{% tab title="Claude Code" %}

Run this command in your terminal:

```bash
claude mcp add --scope user --transport stdio shopify-dev-mcp -- npx -y @shopify/dev-mcp@latest
```

Restart Claude Code to load the server.

{% endtab %}

{% tab title="Claude Desktop / Cursor / Gemini CLI" %}

Add this configuration to your MCP servers:

```json
{
  "mcpServers": {
    "shopify-dev-mcp": {
      "command": "npx",
      "args": ["-y", "@shopify/dev-mcp@latest"]
    }
  }
}
```

Save and restart your tool.

{% endtab %}

{% tab title="Codex CLI" %}

Add this configuration to your `~/.codex/config.toml` file:

```toml
[mcp_servers.shopify-dev-mcp]
command = "npx"
args = ["-y", "@shopify/dev-mcp@latest"]
```

Restart Codex to load the server.

{% endtab %}
{% endtabs %}

***

## Available tools

The Shopify Dev MCP server provides:

* **`introspect_graphql_schema`** — explore GraphQL types, fields, and operations
* **`validate_graphql_codeblocks`** — validate GraphQL queries and mutations against the schema
* **`search_docs_chunks`** — search across all shopify.dev documentation
* **`fetch_full_docs`** — retrieve complete documentation pages
* **`learn_shopify_api`** — introduction to Shopify's supported APIs

***

## Using with the Mechanic MCP server

For the best Mechanic development experience, run both MCP servers together:

* **Mechanic MCP** — task library search, Mechanic documentation, task code examples
* **Shopify Dev MCP** — GraphQL schema, API docs, query validation

Your AI assistant will use both servers automatically when answering Mechanic-related questions.

***

## Related resources

* [Mechanic MCP Server](mcp.md) — connect your AI to Mechanic's task library and docs
* [Shopify Dev MCP documentation](https://shopify.dev/docs/apps/build/devmcp) — Shopify's official setup guide
* [Using AI with Mechanic](../ai.md) — overview of all Mechanic AI tools
