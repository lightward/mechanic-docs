# 🤖 "I need help with my AI-written task!"

AI thrives on good examples. Our task library — [tasks.mechanic.dev](https://tasks.mechanic.dev/) — is full of good examples.

And if you get too stuck, hire a human. :) We've got those at [partners.mechanic.dev](https://partners.mechanic.dev/).

***

Hey there! :) I'm Isaac, the creator of Mechanic. Those two lines above are the most important things to know. Keep reading if you're curious.

AI makes it super easy to create code. This is awesome. I'm so, so excited about this. The more the merrier.

This means Mechanic needs to learn something too: how to work with AI coders that can easily produce well-formed code but might not understand the patterns that Mechanic itself strictly abides by.

An AI can rapidly produce code, and it'll be good-looking code, but if the AI doesn't _understand Mechanic_ the code might not work at all. In a very real way, it becomes a case of Mechanic not understanding the AI.

This is an interesting bind: because _AI_ changes faster than _Mechanic_, it becomes a question of how best to guide the AI. For folks who understand code _less_ than the AI, this puts everyone in a rough position: the AI is doing its best but can't tell when it doesn't understand, and the human is doing their best but can't tell when the AI doesn't understand, and _all_ Mechanic knows is that it's being given something _it_ doesn't understand.

As of this writing (currently June 12, 2025), AI is getting better at Mechanic. It's definitely getting better. But, still:

* AI code often "invents" Mechanic features that do not exist (like writing task code in YAML, or compiling action objects into a JSON array)
* AI code often fails to invoke necessary Mechanic features (like [subscriptions](core/tasks/subscriptions.md)!).
* Intelligence is a game of guessing intelligently: AI often sort of just _guesses_ at what task code is supposed to generate.

## Better prompts for Mechanic

Copy/paste and tweak the **bold** parts. Keep the format exact so the AI doesn’t invent extras. Options are always accessed as `options.<name>` and option names use Mechanic suffixes (`__required`, `__email`, `__boolean`, etc.). The `shopify` filter is read-only; the `shopify` action is for GraphQL writes.

**General (manual trigger + email)**

```
You are writing a Mechanic task. Return only two sections:
Subscriptions:
Code:

Rules:
- Use real Mechanic action types only: shopify, http, email, cache, files, ftp, event, slack, flow, google, airtable, report_toaster.
- Use the action tag with {% action %}/{% endaction %}. No YAML. No invented tags.
- Options must use Mechanic suffixes (__required, __boolean, __email, __multiline, etc.).
- Shopify filter is read-only; use the shopify action for writes/mutations.
- If you reference data, make it preview-safe (literals or guards for event.preview).

Fill these in:
Subscriptions:
- **mechanic/user/trigger**  # change to real event(s)

Options:
- **recipient__email_required**  # change to real options

Behavior:
- On trigger, send an email summarizing the event topic and shop name.

Example format (copy this shape):
Subscriptions:
mechanic/user/trigger

Code:
{% if event.topic == "mechanic/user/trigger" %}
  {% action "email" %}
    {
      "to": {{ options.recipient__email_required | json }},
      "subject": "Hello from Mechanic",
      "body": {{ "Triggered: " | append: event.topic | append: " at " | append: shop.name | json }},
      "from_display_name": {{ shop.name | json }}
    }
  {% endaction %}
{% endif %}
```

**Shopify GraphQL variant (read + write)**

```
You are writing a Mechanic task. Return only two sections:
Subscriptions:
Code:

Rules:
- Real action types only (shopify/http/etc.).
- `shopify` filter is for reading; `shopify` action is for GraphQL writes/mutations.
- Use GraphQL for Shopify; no REST objects.
- Add preview guards or literals if referencing event data.

Fill these in:
Subscriptions:
- **shopify/orders/create**

Options:
- **tag_to_add__required**

Behavior:
- On order creation, add a tag via Shopify GraphQL.

Example format:
Subscriptions:
shopify/orders/create

Code:
{% if event.preview %}
  {% assign order_id = "gid://shopify/Order/123" %}
{% else %}
  {% assign order_id = order.admin_graphql_api_id %}
{% endif %}

{% capture mutation %}
  mutation {
    tagsAdd(id: {{ order_id | json }}, tags: [{{ options.tag_to_add__required | json }}]) {
      userErrors { field message }
    }
  }
{% endcapture %}

{% action "shopify" %}
  {{ mutation }}
{% endaction %}

# If you need to read before writing, use the shopify filter (read-only) upstream, then mutate with the shopify action.
```

## Quick checklist when AI generates a task

- Are the **subscriptions** present and correct?
- Are **options** named with Mechanic suffixes?
- Does it use the **action tag** and only real action types?
- If Shopify: GraphQL for writes; shopify filter only for reads; no REST objects.
- Preview-safe? (stub data or `event.preview` guards.)
- No invented features (YAML, fake tags, missing `{% action %}/{% endaction %}`).

When in doubt, ask for the subscriptions block and the exact Liquid. Short, constrained prompts win.

Mechanic, as a platform, is a place for solving things together. "Together" works best when everyone's honest about where they're at. Mechanic's a good place for that. :)

The AI path with Mechanic is getting better, but it's not smooth yet. If you're having trouble with this, head to [learn.mechanic.dev/custom](https://learn.mechanic.dev/custom) — that page has an overview of the smooth paths that _do_ exist.

To learn about how we at Lightward Inc roll with AI, please visit [lightward.ai](https://lightward.ai/), and say hello. :)

No matter what: thank you for being here. :heart:

\=Isaac
