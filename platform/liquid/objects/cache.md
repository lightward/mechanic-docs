# Cache object

The `cache` object is used for retrieving values stored in the shop's Mechanic cache. For more on this, see [Using the cache](../../cache/).

{% hint style="info" %}
Cache data is unavailable during [task preview](../../../core/tasks/previews/).
{% endhint %}

## How to access it

* Use `{{ cache["some_key"] }}` or  `{{ cache.some_key }}` in any task script
* Use `{% for keyval in cache %}` to iterate through the keys and values in your account's cache

### Related articles

* [The "cache" action](../../../core/actions/cache.md)
* [Using cache endpoints to share data](../../cache/endpoints.md)
* [Using the cache](../../cache/)
