# Mechanic tags

Mechanic extends Liquid with custom tags that go beyond what standard Liquid provides. These tags are how tasks produce output — defining actions, logging data, raising errors, and more.

## Available tags

* [action](action.md) — defines an [action](../../../core/actions/) to be performed after the task finishes rendering (e.g. a Shopify API call, an email, an HTTP request)
* [log](log.md) — outputs structured data to the task run's log, useful for debugging and auditing
* [error](error.md) — raises an error that halts the task run and marks it as failed
* [assign](assign.md) — an enhanced version of Liquid's assign tag, with support for nested hash key assignment
* [permissions](permissions.md) — explicitly declares Shopify access scopes the task requires, supplementing Mechanic's automatic permission detection
* [liquid](liquid.md) — evaluates a string as a Liquid template at runtime, useful for dynamic template rendering
