# User

Mechanic allows developers several ways to generate custom event topics, all within the User domain.

* The [Event action](../../actions/event.md) can be used with any User event topic
* [Webhooks](../../../advanced-topics/webhooks.md) may be configured to generate events using any User event topic

An event in the User domain must use the standard three-part topic form, but only the "user/" prefix is mandatory. The subject and verb of the event topic may be chosen by the developer \(e.g. "user/foo/bar"\).

