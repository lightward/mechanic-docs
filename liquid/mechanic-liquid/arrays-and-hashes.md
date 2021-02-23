# Arrays and hashes

Mechanic's Liquid additions include mutable arrays and hashes, via the`assign`tag. These additions provide access to the power of Ruby, which backs the Liquid template language. Arrays and hashes are key to developing more complex tasks.

To facilitate this, we introduce two new keyword literals:

* `hash`  – `{}` 
* `array`  – `[]` 

## Arrays

* You may assign within an array by index, using integer lookups.
* Assignment within arrays is always by value, not by reference.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign foo = array %}
{{ foo | json }}

{% assign foo[0] = "bar" %}
{{ foo | json }}

{% assign baz = "foo,bar" | split: "," %}
{{ baz | json }}

{% assign baz[2] = "baz" %}
{{ baz | json }}
```
{% endtab %}

{% tab title="Output" %}
```
[]
["bar"]
["foo","bar"]
["foo","bar","baz"]
```
{% endtab %}
{% endtabs %}

## Hashes

The term comes from Ruby, the language beneath Liquid. In Ruby \([docs](https://ruby-doc.org/core-2.5.1/Hash.html)\), "A Hash is a dictionary-like collection of unique keys and their values." In Mechanic's Liquid implementation, a hash can only have string keys. The below example uses our `parse_json` filter, which turns a JSON string into an object,  learn more [here]().

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign foo = hash %}
{{ foo | json }}

{% assign foo["bar"] = "baz" %}
{{ foo | json }}

{% assign baz = '{"qux": "quux"}' | parse_json %}
{% assign foo["bar"] = baz %}
{{ foo | json }}

```
{% endtab %}

{% tab title="Output" %}
```
{}
{"bar":"baz"}
{"bar":{"qux":"quux"}}

```
{% endtab %}
{% endtabs %}

