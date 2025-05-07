# Options

[**Tasks**](https://learn.mechanic.dev/core/tasks) accept user configuration via **options**. Options are created dynamically, by reference: each option referenced in a task's [**code**](https://learn.mechanic.dev/core/tasks/code) results in that option being added to the task's configuration form. In the option reference `{{ options.foo_bar__required }}`, the option **key** is `foo_bar__required`. The appearance and behavior of the option's form element is based on **flags** in in the option key – in this example, only the "required" flag is in use.

Mechanic flags provide only limited option validation. A task may define [**custom validation**](https://learn.mechanic.dev/core/tasks/options/custom-validation), by rendering error objects according to the task's its own validation logic.

## 1. Keys

Options are made available in the `options` variable, which is a hash having key-value pairs for each option key and option value. The option key must only contain ASCII numbers, lowercase letters, and underscores. The option key is reformatted for use as the option name presented to the user – underscores are replaced by spaces, and the entire line is sentence-cased.

```
<name>[__<flag>[ _<flag> ... ]]
```

| Part   | Rules                                                    | Example                              |
| ------ | -------------------------------------------------------- | ------------------------------------ |
| `name` | Lowercase letters, numbers, and underscores only.        | `send_after`                         |
| `flag` | One or more tokens that customise the field (see below). | `required`, `date`, `picker_product` |

Because option keys are registered via static analysis, options must each be referenced using a standard lookup (e.g. `options.foobar`) at least once.

***

## 2. Display Order

Options are displayed to the user in the order in which they are first referenced in the task code.

Because this may not result in a natural sequence, it can be useful to prefix task code with a comment block, explicitly referencing each option so as to force the overall order.

```liquid
{% raw %}
{% comment %}
  Option order:

  {{ options.api_key__required }}
  {{ options.mode__select_o1_test_o2_live }}
  {{ options.webhooks__array }}
{% endcomment %}
{% endraw %}
```

***

## 3. Flags

Option flags control how an option appears and behaves in a task's configuration form, and also control the type and format of the option value.

Many flags may be combined with other flags, for more nuanced control.

If no flags are used for an option, an option will be made available as a plain text field, and the option value will be a string.

#### Flags fall into three categories:

| Category            | Purpose                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Input types**     | Choose a specific control and value type (date picker, slider, select list, …). Exactly **one** input‑type flag should be used. |
| **Form modifiers**  | Fine‑tune how the control looks or validates (required, multiline, etc.).                                                       |
| **Auxiliary flags** | Extra behaviour for certain input types (future‑only dates, multi‑select choices, etc.).                                        |

### 3.1 Input‑type flags

<table><thead><tr><th>Flag</th><th>UI control</th><th width="197.44140625">Value returned</th><th>Key examples</th></tr></thead><tbody><tr><td> <em>(none - default)</em></td><td>Single‑line text</td><td><code>string</code></td><td><code>options.subject</code></td></tr><tr><td><code>multiline</code></td><td>Multiline text box</td><td><code>string</code></td><td><code>options.body__multiline</code></td></tr><tr><td><code>boolean</code></td><td>Checkbox</td><td><code>true/false</code></td><td><code>options.enabled__boolean</code></td></tr><tr><td><code>number</code></td><td>Numeric input (<code>step=1</code>)</td><td><code>number</code></td><td><code>options.count__number</code></td></tr><tr><td><code>code</code></td><td>Code‑formatted text area</td><td><code>string</code></td><td><code>options.script_snippet__code_multiline</code></td></tr><tr><td><code>keyval</code></td><td>Key → value repeater</td><td><code>hash</code></td><td><code>options.headers__keyval</code></td></tr><tr><td><code>array</code></td><td>Value repeater</td><td><code>array</code></td><td><code>options.tags__array</code></td></tr><tr><td><code>date</code></td><td>Calendar picker</td><td><code>"YYYY‑MM‑DD"</code></td><td><code>options.launch_date__date</code></td></tr><tr><td><code>datetime</code></td><td>Date+time picker</td><td><code>"YYYY‑MM‑DDTHH:MM"</code></td><td><code>options.send_at__datetime</code></td></tr><tr><td><code>time</code></td><td>Time‑only picker</td><td><code>"HH:MM"</code></td><td><code>options.quiet_time__time</code></td></tr><tr><td><code>color</code></td><td>Hex colour picker</td><td><code>"#RRGGBB"</code></td><td><code>options.theme_color__color</code></td></tr><tr><td><code>range_minX_maxY_stepZ</code></td><td>Slider + number box</td><td><code>number</code></td><td><code>options.qty__range_min0_max100_step5</code></td></tr><tr><td><code>select</code></td><td>Single‑choice dropdown</td><td><code>string</code></td><td><code>options.plan__select_o1_basic_o2_pro</code></td></tr><tr><td><code>choice</code></td><td>Radio buttons</td><td><code>string</code></td><td><code>options.tier__choice_o1_gold_o2_silver</code></td></tr><tr><td><code>multiselect</code></td><td>Checkbox list</td><td><code>array[string]</code></td><td><code>options.channels__multiselect_o1_email_o2_sms</code></td></tr><tr><td><code>picker_&#x3C;resource></code></td><td>Shopify resource picker</td><td><code>gid string</code></td><td><code>options.product__picker_product</code></td></tr><tr><td><code>picker_&#x3C;resource>_array</code></td><td>Multi‑select resource picker</td><td><code>array[gid]</code></td><td><code>options.products__picker_product_array</code></td></tr></tbody></table>

{% hint style="info" %}
Array options have a hidden feature: once the user-configured array reaches 5 elements in size, a new "Manage in bulk" button will appear for that option. Clicking it will open a modal which allows the user to manage the array's input using a single multiline text field, in which each line represents an array element. This is a convenient way to configure larger arrays.
{% endhint %}

#### Choice/Select grammar

Use ordinal tokens to define values:

```
<name>__select_o1_basic_o2_pro_o3_enterprise
               │  │     │  │
               │  │     │  └───── value #2
               │  │     └──────── ordinal marker
               │  └──────────── value #1
               └─────────────── ordinal marker
```

Ordinals (`o1`, `o2`, …) determine display order. Underscores inside values are preserved.

#### Range grammar

* `min<number>` – **required**
* `max<number>` – **required**
* `step<number>` – optional (defaults to `1`).

#### Picker grammar

```
<name>__picker_<product|variant|collection>[_array]
```

* Append `_array` for multi‑select.
* Unsupported resources are rejected during validation.

### 3.2 Form‑modifier flags

| Flag       | Applies to | Effect                                               |
| ---------- | ---------- | ---------------------------------------------------- |
| `required` | Any        | Field must be filled before Save.                    |
| `email`    | `text`     | Adds email placeholder and basic email format check. |

### 3.3 Auxiliary flags

| Flag         | Works with         | Effect                       |
| ------------ | ------------------ | ---------------------------- |
| `futureonly` | `date`, `datetime` | Picker disallows past dates. |

***

## 4. Built‑in validation

1. **Required fields** must not be empty.
2. **Range sliders** need both `min` and `max`.
3. **Pickers** only allow `product`, `variant`, or `collection`.
4. **Email** inputs are matched against a basic regex.

Custom rules? Learn more about [custom validation](custom-validation.md).

***

## 5. Liquid

Options that allow text input are evaluated for Liquid when a task processes an event. Liquid evaluation for options occurs before it occurs for task code, which means that any Liquid variables created by task code are not available to task options.

Liquid code in task options have access to the same set of [environment variables](https://learn.mechanic.dev/core/tasks/code/environment-variables) that are made available to the task code, including `event`, `shop`, `cache`, and any event subject variables.

***

## 6. Quick reference

| Goal           | Key snippet                                     | Value example                  |
| -------------- | ----------------------------------------------- | ------------------------------ |
| Text input     | `options.subject__required`                     | `"Welcome!"`                   |
| Email input    | `options.reply_to__email_required`              | `"person@example.com"`         |
| Checkbox       | `options.newsletter__boolean`                   | `false`                        |
| Key–value map  | `options.headers__keyval`                       | `{ "X-Env": "staging" }`       |
| String list    | `options.tags__array`                           | `["vip","wholesale"]`          |
| 0–100 slider   | `options.score__range_min0_max100`              | `42`                           |
| Colour picker  | `options.bg__color`                             | `"#336699"`                    |
| Dropdown       | `options.plan__select_o1_basic_o2_pro`          | `"basic"`                      |
| Multi‑select   | `options.channels__multiselect_o1_email_o2_sms` | `["email","sms"]`              |
| Product picker | `options.promo__picker_product`                 | `"gid://shopify/Product/123"`  |
| Product list   | `options.products__picker_product_array`        | `[ "gid://…/1", "gid://…/2" ]` |
| Date           | `options.go_live__date_required`                | `"2025-05-06"`                 |
| Time           | `options.quiet_at__time`                        | `"00:25"`                      |
| Datetime       | options.party\_\_datetime                       | `"2031-04-22T15:13:00"`        |

### Working with date options

Getting a plain string (strip the offset)

```
{{ options.launch_date__date | date: "%Y-%m-%d" }}
   ⇒ 2025-05-06

{{ options.quiet_at__time  | date: "%H:%M" }}
   ⇒ 00:25

{{ options.party__datetime     | date: "%Y-%m-%d %H:%M" }}
   ⇒ 2031-04-22 15:13
```

#### Converting to another timezone

```
{{ options.party__datetime | date: tz: "UTC" }}
   ⇒ 2031-04-22T19:13:00Z       {offset shifted +4 h}

{{ options.quiet_at__time | date: "%H:%M %Z", tz: "America/Vancouver" }}
   ⇒ 21:25 PDT
```
