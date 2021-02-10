# Conditionals

## if tag

```javascript
{% if options.test_mode__boolean %}
  {% action "echo" summaries %}
{% endif %}
```

## else tag

```javascript
{% if tags_to_add != empty %}
  {% assign tag = tags_to_add %}
{% else %}
  {% assign tag = "tag was empty" %}
{% endif %}
```

## elsif tag

```javascript
{% if event.topic == "mechanic/scheduler/daily" and options.next_day__boolean %}
  {% assign proceed = true %}
{% elsif event.topic == "mechanic/user/trigger" and options.on_demand__boolean %}
  {% assign proceed = true %}
{% endif %}
```

## unless tag

Similar to a traditional `if` tag, but only executes if the condition results in `false`.

```javascript
{% unless options.default_tracking_url_template contains "TRACKING_NUMBER" %}
  {% error %}{{ "Placeholder 'TRACKING_NUMBER' is missing." | json }}{% enderror %}
{% endunless %}
```

### case / when tag <a id="case-when"></a>

```javascript
{% case order.cancel_reason	%}
  {% when 'customer' %}
     {% assign cancel_reason = "It was the customer." %}
  {% when 'fraud' %}
    {% assign cancel_reason = "It was fraud!" %}
  {% when 'inventory' %}
    {% assign cancel_reason = "It was due to inventory issues." %}
  {% else %}
     {% assign cancel_reason = "It was none of the above." %}
{% endcase %}
```

