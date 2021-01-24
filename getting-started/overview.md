# Overview

Mechanic has four core concepts: events, tasks, actions, and – beneath all of these – runs. In their natural order:

1. An [_event_](https://docs.usemechanic.com/article/414-an-introduction-to-events) occurs, usually triggered by something in Shopify.
2. Mechanic renders whatever [_tasks_](https://docs.usemechanic.com/article/417-an-introduction-to-tasks) apply to that event.
3. Mechanic executes whatever [_actions_](https://docs.usemechanic.com/article/415-an-introduction-to-actions) those tasks define.
4. Every event, task, and action has a [_run_](https://docs.usemechanic.com/article/425-an-introduction-to-runs), representing the effort that Mechanic takes to do the work behind that event, task, or action.

\(See the "Core concepts" section below for a more in-depth review.\)

Importantly, we also have Liquid. You'll encounter this pretty much everywhere in Mechanic: it's a template language created by Shopify, and we use it heavily here, with a few extensions of our own. [Read more about Liquid at Mechanic](https://docs.usemechanic.com/article/451-an-introduction-to-liquid-at-mechanic) – strongly recommended reading. :\)

Finally, if you're reading this, you're probably interested in reading from and writing to Shopify. Skipping ahead just a bit: you'll read data using Liquid \(see [Querying Shopify](https://docs.usemechanic.com/article/358-querying-shopify)\), and you'll write data using [the "shopify" action](https://docs.usemechanic.com/article/420-the-shopify-action).

