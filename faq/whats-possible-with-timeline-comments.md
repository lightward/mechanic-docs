# What's possible with timeline comments?

## What's possible

* Reading timeline [comments](https://shopify.dev/docs/admin-api/graphql/reference/events/commentevent), and [other events](https://shopify.dev/docs/admin-api/graphql/reference/events/basicevent), for orders, draft orders, customers, and inventory transfers.
* Writing order and customer _notes_. This is _not the same_ as the timeline comments. We mention it here because this frequently makes for an acceptable fallback approach.

## What's not possible

* Using a timeline event as a Mechanic event. This means that Mechanic tasks can't run in direct response to a new comment \(or other timeline event\).
* Reading the name of the staff person who left a comment, or the name of the app responsible for another event.
* Creating, updating, prefilling, or deleting timeline comments.

