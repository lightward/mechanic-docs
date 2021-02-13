# Arrays and Hashes

Mechanic's Liquid additions include mutable arrays and hashes, via the`assign`tag. These additions provide access to the power of Ruby, which backs the Liquid template language.

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



