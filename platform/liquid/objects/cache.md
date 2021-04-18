# Cache object

The `cache` object is used for retrieving values stored in the shop's Mechanic cache. For more on this, see [Using the cache](../../mechanic-cache/).

## How to access it

* Use `{{ cache["some_key"] }}` or  `{{ cache.some_key }}` in any task script
* Use `{% for keyval in cache %}` to iterate through the keys and values in your account's cache

### Related articles

* [The "cache" action](../../../core-concepts/actions/types/cache.md)
* [Using cache endpoints to share data](../../mechanic-cache/cache-endpoints.md)
* [Using the cache](../../mechanic-cache/)

