# Syntax

Liquid is a template language, which means that it uses special syntax to mark the places where Liquid code starts and ends. In this way, Liquid code can be used to fill in calculated values in a larger document.

In Shopify, Liquid is usually found in HTML templates. In Mechanic, Liquid is usually found in JSON templates.

## Liquid with output

When using the `{{ code }}` syntax, the result of the Liquid code inside will form output.

In the following example, this syntax is used to output the string `"world"`. When this template is rendered, it will produce `Hello, world`.

```javascript
Hello, {{ "world" }}
```

## Liquid without output

When using the \`

{% code title="" %}
```

```
{% endcode %}

\` syntax, the Liquid code inside is given the opportunity to perform work without generating output. This syntax is for preparing and modifying [variables](variables.md) \(using tags like [assign](../tags/assign.md)\), or for specifying [control flow](control-flow/) \(using [conditions](control-flow/condition.md) or [iteration](control-flow/iteration.md)\).

In the following example, a variable is assigned, modified with a new variable, and is finally rendered as output.

```javascript
{% assign scope = "world" %}
{% assign message = "Hello, " | append: scope %}

{{ message }}
```

