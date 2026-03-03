# 🤖 Using AI with Mechanic

{% hint style="info" %}
Mechanic offers dedicated AI tools that understand the platform's patterns, task library, and documentation:

* **[Mechanic MCP Server](platform/mcp.md)** — connect your AI coding tool to the task library and docs
* **[Mechanic Agent Skills](platform/agent-skills.md)** — install task-writing expertise into your AI coding tool
* **[Mechanic Sidekick Skill](platform/sidekick.md)** — write tasks with Shopify's built-in AI assistant
* **[Mechanic ChatGPT](platform/chatgpt.md)** — write tasks and get help in ChatGPT
{% endhint %}

***

## AI tools for Mechanic

### For developers

The **[MCP server](platform/mcp.md)** and **[Agent Skills](platform/agent-skills.md)** are designed to work together. The MCP server gives your AI assistant access to Mechanic's task library and documentation at query time. Agent Skills install Mechanic's task-writing patterns directly into your AI's context, so it knows how to structure subscriptions, handle preview mode, use GraphQL, and produce complete importable task JSON.

For the best results, use both: the MCP server for research and reference, and Agent Skills for writing code.

The **[Shopify Dev MCP Server](platform/shopify-dev-mcp.md)** is also worth adding — it gives your AI assistant direct access to Shopify's GraphQL schema and query validation.

If you're working in Shopify Admin, the **[Sidekick skill](platform/sidekick.md)** brings Mechanic task-writing expertise directly into Shopify's built-in AI assistant.

### For ChatGPT users

The **[Mechanic ChatGPT](platform/chatgpt.md)** can write tasks, debug existing ones, and answer general Mechanic questions — all within ChatGPT. It's a good option if you prefer a conversational interface over a coding tool.

***

## Support for AI-generated tasks

Tasks created with AI assistance are [custom tasks](custom.md) and follow the same support policy. The Mechanic team provides support for the platform itself but not for debugging custom task logic.

If you need help with a custom task, you can:

* Ask in the [Mechanic Slack community](resources/slack.md)
* Hire a developer through [partners.mechanic.dev](https://partners.mechanic.dev/)

***

## Tips for working with AI

These apply regardless of which AI tool you're using:

* **Start from the task library.** Search [tasks.mechanic.dev](https://tasks.mechanic.dev/) for an existing task that's close to what you need. Modifying a working task is faster and more reliable than starting from scratch.
* **Check subscriptions.** Make sure the task subscribes to the right event topics. A task that listens to `shopify/orders/create` won't fire on order updates.
* **Prefer GraphQL.** Shopify REST is deprecated in Mechanic. Use the GraphQL Admin API for reading and writing Shopify data.
* **Verify the output format.** A complete Mechanic task is a JSON object with `name`, `script`, `subscriptions`, `subscriptions_template`, `options`, and other fields. If your AI produces only a Liquid script, it's missing the structure needed for import.
