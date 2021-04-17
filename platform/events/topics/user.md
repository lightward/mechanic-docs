---
description: An introduction to user-defined topics
---

# User

The User event domain is for custom, user-generated events, having any subject and verb \(e.g. "user/foo/bar"\). As with all events, a User event topic must use the standard three-part topic form, but only the "user/" prefix is mandatory.

Mechanic allows developers several ways to generate custom User events:

* The [Event action](../../../core-concepts/actions/action-types/event.md) can be used with any User event topic
* [Webhooks](../../webhooks.md) may be configured to generate events using any User event topic

