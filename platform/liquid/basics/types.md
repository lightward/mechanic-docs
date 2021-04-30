# Data types

In Liquid, different kids of data have different **types**. Each type describes the nature of its data.

## String

A string contains a series of characters, forming text.

```java
{% assign my_name = "Matt" %}
```

## Integer, Float

Liquid supports both two number types: integers \(whole numbers\) and floats \(numbers having decimal precision\).

```java
{% assign a_int = 99 %}
{% assign a_float = 99.99 %}
```

## Boolean

```java
{% assign is_mechanic_awesome = true %}
{% assign is_it_warm_outside = false %}
```

## Nil

Borrowing from [Ruby's concept of nil](https://www.rubyguides.com/2018/01/ruby-nil/), Liquid's `nil` is an empty value that is returned when Liquid code has no results. It evaluates to `false` in conditionals statements, and outputs nothing when printing out text.

```java
{% if order.email %}
  There is an email address
{% endif %}
```

{% tabs %}
{% tab title="Liquid" %}
```java
email address: {{ order.email }}
```
{% endtab %}

{% tab title="Output when not nil" %}
```text
email address: joe@joegmail.com
```
{% endtab %}

{% tab title="Output when nil" %}
```text
email address:
```
{% endtab %}
{% endtabs %}

## Array

An array is a value that itself contains an ordered list of other values. Each value has an index, representing the order in which each value occurs in the list.

### Creating an array

Liquid supports creating arrays of strings using the [split](../filters.md#split) filter. In Mechanic, arrays can be created using the [array](types.md) literal.

### Iterating through arrays

```java
{% for customer_tag in customers.tags %}
  {{ customer_tag }}
{% endfor %}
```

### Accessing an array element

```java
{{ customers.tags[0] }}
{{ customers.tags[1] }}
```

### Array filters

Mechanic includes a variety of [array filters](../filters.md#array-filters), useful for transforming arrays or retrieving specific values.

## Object

An **object** is any value that has attributes \(also known as properties\). The name of an attribute is known as its **key**; the data stored for an attribute is known as its **value**. In Mechanic, some objects have additional intelligence of their own, like the `shop` object.

### Iterating through objects

Objects may be traversed using [for loops](control-flow/iteration.md).

```javascript
{% assign object = hash %}
{% assign object["foo"] = "bar" %}
{% assign object["baz"] = "qux" %}

{% for keyval in object %}
  {% assign key = keyval[0] %}
  {% assign value = keyval[1] %}

  {{ key }}: {{ value }}
{% endfor %}
```

### Accessing an object property

All of the following examples return the same value.

```javascript
{% assign foo = '{"foo":"bar"}' | parse_json %}

1. {{ foo.bar }}
2. {{ foo["bar"] }}

{% assign key = "bar" %}
3. {{ foo[key] }}
```

## Hash

In Mechanic, a **hash** is a simple type of object that has no additional intelligence at all; it only contains keys and values. It can be constructed by the developer using code.

### Creating a hash

In Mechanic, hashes can be created using the [hash](../keyword-literals/hash.md) literal, or by using any of the "parse" [filters](../filters.md).

{% hint style="info" %}
This only applies to Mechanic. Liquid objects and hashes cannot be created in Shopify.
{% endhint %}

### Iterating through hashes

Hashes may be traversed using [for loops](control-flow/iteration.md), like other objects. For convenience, Mechanic also supports extract an array of hash keys using the [keys](../filters.md#keys) filter, or object values using the [values](../filters.md#values) filter. The resulting arrays may also be used with a for loop, like any array.

```javascript
{% assign object = hash %}
{% assign object["foo"] = "bar" %}
{% assign object["baz"] = "qux" %}

{% for keyval in object %}
  {% assign key = keyval[0] %}
  {% assign value = keyval[1] %}

  {{ key }}: {{ value }}
{% endfor %}

{% assign keys = object | keys %}
{% for key in keys %}
  {{ key }}: {{ object[key] }}
{% endfor %}

{% assign values = object | values %}
{% for value in values %}
  {{ value }}
{% endfor %}
```

