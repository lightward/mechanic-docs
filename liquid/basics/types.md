# Types

## String

```java
{% assign my_name = "Matt" %}
```

## Number

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

`Nil` is an empty value that is returned when Liquid code has no results. It evaluates to `false` in `if` statements and outputs nothing when printing out text.  


```java
{% if order.email %}
  There is an email address
{% endif %}
```

{% tabs %}
{% tab title="Code" %}
```java
email address: {{ order.email }}
```
{% endtab %}

{% tab title="Output when not nil" %}
```
email address: joe@joegmail.com
```
{% endtab %}

{% tab title="Output when nil" %}
```
email address:
```
{% endtab %}
{% endtabs %}

## Array

In base Liquid, you can't initialize arrays with only Liquid. We've made additions to the language to help with this. In the current section, we'll cover arrays as they exist in the base Liquid language.

### Iterating through arrays

```java
{% for customer_tag in customers.tags %}
  {{ customer_tag }}
{% endfor %}
```

### Accessing an array item

```java
{{ customers.tags[0] }}
{{ customers.tags[1] }}

```

### Array filters

Check out the [array filters section](basic-liquid-filters.md#array-filters) to learn about:

`join, first, last, concat, index, map, reverse, size, sort, where, uniq.`

