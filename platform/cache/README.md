---
description: "Store and retrieve data between Mechanic task runs using the key-value cache — with configurable expiration up to 60 days."
---

# Cache

Each Mechanic account has a simple key-value cache, which may be written to using [Cache](../../core/actions/cache.md) actions, and read from using the [cache object](../liquid/objects/cache.md) and [cache endpoints](endpoints.md).

{% hint style="info" %}
Cache data is unavailable during [task preview](../../core/tasks/previews/).
{% endhint %}

## Restrictions

Cache keys must match `/\A[a-z0-9_:-.\/]+\z/i`; that is, they must only contain alphanumeric characters, plus any of the following characters: `_:-./`.

The cache is intended for temporary data storage. As such, each stored value is persisted for a maximum of 60 days. (Shorter expiration periods can be set using the Cache action's "ttl" option.)

The cache is not intended for large value storage. As such, each stored value is limited to a maximum of 256kb.

For storing larger values, consider spreading the value across multiple cache keys, or using the [FTP](../../core/actions/ftp.md) action to upload data to another storage service (possibly using a service like [Couchdrop](https://couchdrop.io/) as a intermediary). To read larger values in task code, use the [HTTP](../../core/actions/http.md) action to perform a GET request, then [respond to the action's results](../../techniques/responding-to-action-results.md).

## Common patterns

The cache is a versatile tool — here are some of the ways it's commonly used:

* [Debouncing events](../../techniques/debouncing-events.md) — use the cache to skip duplicate events that arrive in rapid succession
* [Batch processing of events](../../techniques/batch-processing-of-events.md) — accumulate data in the cache across multiple events, then process it all at once on a schedule
* [Responding to action results](../../techniques/responding-to-action-results.md) — store state across the multiple task runs in a multi-step workflow
