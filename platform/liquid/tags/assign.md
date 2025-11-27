# assign

The **assign** tag is a native feature of Liquid. In Mechanic-flavored Liquid, the assign tag is extended to support assigning within [**arrays**](../basics/types.md#array) and [**hashes**](../basics/types.md#hash).

Assignment into arrays and hashes is always by value, never by reference.

{% hint style="info" %}
"Assignment by value" means that the result of the assignment will never dynamically change.

```
{% assign foo = "bar" %}
{% assign x = array %}
{% assign x["foo"] = foo %}

{% assign foo = "qux" %}
```

At the end of this example, `x.foo` still contains `"bar"`, even though the value of the original `foo` variable has changed.
{% endhint %}

## Assigning into arrays

Arrays support assignment by index, using integer lookups.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign x = array %}
{% assign x[0] = "one" %}
{% assign x[x.size] = "two" %}

{% assign the_third_zero_based_index = 2 %}
{% assign x[the_third_zero_based_index] = "three" %}

{{ x | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
["one","two","three"]
```
{% endtab %}
{% endtabs %}

## Assigning into hashes

Hashes support assignment by key, using string lookups.

{% tabs %}
{% tab title="Code" %}
```javascript
{% assign x = hash %}
{% assign x["one"] = 1 %}
{% assign x["two"] = 2 %}

{% assign three = "three" %}
{% assign x[three] = 3 %}

{{ x | json }}
```
{% endtab %}

{% tab title="Output" %}
```javascript
{"one":1,"two":2,"three":3}
```
{% endtab %}
{% endtabs %}
