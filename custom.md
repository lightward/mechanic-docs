---
description: "Build custom Shopify automations with Mechanic using Liquid, GraphQL, and HTTP — or hire a developer to build one for you."
---

# 🙋 I need something custom!

Mechanic is a development platform – in the hands of a developer, it can be used to accomplish almost anything in Shopify. While our [task library](resources/task-library/) covers many use cases, Mechanic's true strength is in letting developers solve merchant problems quickly, giving merchants easy configuration forms for managing the resulting tasks.

{% hint style="warning" %}
Mechanic support covers the platform and tasks from the [task library](resources/task-library/). Custom tasks (including AI-generated tasks) are not covered by support — for help with custom task logic, ask in the [Mechanic Slack community](resources/slack.md) or [hire a developer](hire-a-developer.md).
{% endhint %}

{% hint style="info" %}
Need something that you think others might need too? The Mechanic community accepts [task requests](resources/task-library/requesting.md), and the top-voted requests are regularly selected for implementation.
{% endhint %}

## If you're using AI

We have tools for this! Mechanic offers an [MCP server](platform/mcp.md) for task library and docs access, [Agent Skills](platform/agent-skills.md) and a [Sidekick skill](platform/sidekick.md) for task-writing patterns, and the [Mechanic GPT](platform/chatgpt.md) for general help in ChatGPT. See [Using AI with Mechanic](ai.md) for the full overview.

## If you need a developer…

Mechanic's automation tasks are written in Liquid, which is a template language used heavily in and around Shopify. This means that developers of all levels, with even a little Shopify development experience, can get started with Mechanic.

**To find a developer for hire, you can contact Mechanic Partners directly at** [**partners.mechanic.dev**](https://partners.mechanic.dev/)**.** This is a growing list of established developers, both independent and agency, who can help you with your implementation.

Lastly: if you already have a developer on your team, or have an existing connection to a developer, send them this article and see if they can help you!

## If you are a developer…

If you're familiar with Liquid and Shopify's Admin APIs, you're ready to build. Mechanic tasks are Liquid code with direct GraphQL access to Shopify, HTTP actions for external APIs, and custom events for composing multi-step workflows. Start by browsing the [task library](resources/task-library/) for patterns close to what you need, then modify or build from scratch using the code editor.

The [Core Concepts](core/events/) section covers the event-task-action model, and [Interacting with Shopify](core/shopify/) covers how tasks read and write Shopify data. The [Techniques](techniques/writing-a-high-quality-task.md) section covers patterns that experienced Mechanic developers use.

Join Mechanic's [Slack workspace](resources/slack.md) to exchange support with the community — the #general channel is a great place to start, and it's filled with people solving problems every day. :)
