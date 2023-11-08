# array

The `array` keyword literal may be used in any Liquid code to instantiate an [array](../basics/types.md#array).

Arrays support [assignment by index](../tags/assign.md#assigning-into-arrays). Many other operations are supported using [array filters](../filters/#array-filters).

## Example

```javascript
{% raw %}
{% assign weekdays = array %}
{% assign weekdays[0] = "Monday" %}
{% assign weekdays[1] = "Tuesday" %}
{% assign weekdays[2] = "Wednesday" %}
{% assign weekdays[3] = "Thursday" %}
{% assign weekdays[4] = "Friday" %}
{% assign weekdays[5] = "Saturday" %}
{% assign weekdays[6] = "Sunday" %}

{% for weekday in weekdays %}
  {{ forloop.index0 }}: {{ weekday }}
{% endfor %}

{% assign weekdays[weekdays.size] = "A NEW WEEKDAY, DYNAMICALLY INDEXED???" %}
{% endraw %}
```
