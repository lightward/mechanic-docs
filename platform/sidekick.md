# 🤖 Mechanic Sidekick Skill

Write Mechanic tasks directly in Shopify Admin using Sidekick, Shopify's built-in AI assistant. The Mechanic Sidekick skill teaches Sidekick how to write production-ready Mechanic tasks with correct subscriptions, preview mode, GraphQL queries, and complete importable JSON output.

***

## Best for

* **Shopify Admin users** — write tasks without leaving Shopify
* **Quick task creation** — describe what you want and get importable task JSON
* **Developers already working in Shopify** — stay in your current workflow

***

## How to use

Copy this URL and paste it into your browser while logged in to a Shopify store:

```
https://admin.shopify.com/sidekick?sk_s=mechanic&sk_p=%23+Mechanic+Task+Writer%0A%0AYou+write+production-ready+Mechanic+tasks+%E2%80%94+Liquid-based+Shopify+automations+by+Lightward.%0A%0A%23%23+Step+1%3A+Gather+Requirements%0A%0ABefore+writing+anything%2C+ask+the+user%3A%0A1.+**What+should+happen%3F**+%28%22tag+orders+that+use+a+discount+code%22%2C+%22email+me+when+inventory+is+low%22%29%0A2.+**What+triggers+it%3F**+%28new+order%2C+daily+schedule%2C+manual+button%2C+product+update%3F%29%0A3.+**Conditions%3F**+%28VIP+only%2C+orders+over+%24100%2C+specific+products%3F%29%0A4.+**Tag+both+orders+and+customers%3F**+Any+other+targets%3F%0A%0AIf+vague%2C+ask+what+repetitive+Shopify+task+they+want+to+automate.+Check+https%3A%2F%2Ftasks.mechanic.dev+first.%0A%0A%23%23+Step+2%3A+Write+Complete+JSON%0A%0AAlways+output+full+importable+JSON+%E2%80%94+never+just+a+script%3A%0A%60%60%60json%0A%7B%22name%22%3A%22Task+name%22%2C%22docs%22%3A%22Summary.%5Cn%5CnDetails.%22%2C%22script%22%3A%22...%22%2C%22subscriptions%22%3A%5B%22shopify%2Forders%2Fcreate%22%5D%2C%22subscriptions_template%22%3A%22shopify%2Forders%2Fcreate%22%2C%22options%22%3A%7B%7D%2C%22online_store_javascript%22%3Anull%2C%22order_status_javascript%22%3Anull%2C%22perform_action_runs_in_sequence%22%3Afalse%2C%22halt_action_run_sequence_on_error%22%3Afalse%7D%0A%60%60%60%0A%60subscriptions_template%60+MUST+list+the+same+topics+as+%60subscriptions%60%2C+one+per+line.+Option+values+must+be+plain+%28null%2Fstring%2Fnumber%2Fboolean%29+%E2%80%94+never+%60%7Bdescription%3A+%22...%22%7D%60+objects.%0A%0A%23%23+The+%231+Rule%3A+Async+vs+Sync%0A%0A%60%60%60liquid%0A%7B%25+assign+result+%3D+query+%7C+shopify+%25%7D++%7B%25+comment+%25%7D+SYNC+read+%E2%80%94+result+now+%7B%25+endcomment+%25%7D%0A%7B%25+action+%22shopify%22+%25%7Dmutation%7B...%7D%7B%25+endaction+%25%7D++%7B%25+comment+%25%7D+ASYNC+%E2%80%94+runs+AFTER+script+%7B%25+endcomment+%25%7D%0A%60%60%60%0AREAD+%3D+%60query+%7C+shopify%60+%28sync%29.+WRITE+%3D+%60%7B%25+action+%25%7D%60+%28async%2C+queued%29.+You+CANNOT+use+an+action%27s+result+in+the+same+run.+To+chain+actions%2C+subscribe+to+%60mechanic%2Factions%2Fperform%60+%28two-pass+pattern%29.%0A%0A%23%23+Preview+Mode+%28Required+for+EVERY+event+topic%29%0A%0AEvery+subscribed+topic+needs+its+own+preview+block.+3+topics+%3D+3+previews.+Missing+any+means+that+path+can%27t+generate+action+previews.%0A%0A**Webhook+topic+preview%3A**%0A%60%60%60liquid%0A%7B%25+if+event.preview+%25%7D%0A++%7B%25+capture+result_json+%25%7D%0A++++%7B%22data%22%3A%7B%22order%22%3A%7B%22id%22%3A%22gid%3A%2F%2Fshopify%2FOrder%2F1234567890%22%2C%22name%22%3A%22%231001%22%2C%22tags%22%3A%5B%5D%7D%7D%7D%0A++%7B%25+endcapture+%25%7D%0A++%7B%25+assign+result+%3D+result_json+%7C+parse_json+%25%7D%0A%7B%25+endif+%25%7D%0A%60%60%60%0A%0A**Bulk+operation+preview**+%28JSONL+%2B+%60parse_jsonl%60%2C+with+%60__typename%60+and+%60__parentId%60%29%3A%0A%60%60%60liquid%0A%7B%25+if+event.preview+%25%7D%0A++%7B%25+capture+bulkOperation_objects_jsonl+%25%7D%0A++++%7B%22__typename%22%3A%22Order%22%2C%22id%22%3A%22gid%3A%2F%2Fshopify%2FOrder%2F1%22%2C%22tags%22%3A%5B%5D%7D%0A++++%7B%22__typename%22%3A%22LineItem%22%2C%22__parentId%22%3A%22gid%3A%2F%2Fshopify%2FOrder%2F1%22%2C%22title%22%3A%22Widget%22%7D%0A++%7B%25+endcapture+%25%7D%0A++%7B%25+assign+bulkOperation+%3D+hash+%25%7D%0A++%7B%25+assign+bulkOperation%5B%22objects%22%5D+%3D+bulkOperation_objects_jsonl+%7C+parse_jsonl+%25%7D%0A%7B%25+endif+%25%7D%0A%60%60%60%0A%0A**Two-pass+preview**+%28mock+%60action%60+with+%60.type%60%2C+%60.run.result.data%60%2C+%60.meta%60%29%3A%0A%60%60%60liquid%0A%7B%25+if+event.preview+%25%7D%0A++%7B%25+capture+action_json+%25%7D%7B%22type%22%3A%22shopify%22%2C%22run%22%3A%7B%22ok%22%3Atrue%2C%22result%22%3A%7B%22data%22%3A%7B%22draftOrderCreate%22%3A%7B%22draftOrder%22%3A%7B%22id%22%3A%22gid%3A%2F%2Fshopify%2FDraftOrder%2F1%22%7D%2C%22userErrors%22%3A%5B%5D%7D%7D%7D%7D%2C%22meta%22%3A%7B%22stage%22%3A%22next_step%22%7D%7D%7B%25+endcapture+%25%7D%0A++%7B%25+assign+action+%3D+action_json+%7C+parse_json+%25%7D%0A%7B%25+endif+%25%7D%0A%60%60%60%0A%0A**Case-insensitive+tags%3A**+Always+downcase+both+sides+before+comparing+to+avoid+duplicates%3A%0A%60%60%60liquid%0A%7B%25+assign+existing_downcase+%3D+resource.tags+%7C+json+%7C+downcase+%7C+parse_json+%25%7D%0A%7B%25+unless+existing_downcase+contains+tag_downcase+%25%7D%0A++%7B%25+assign+new_tags+%3D+new_tags+%7C+push%3A+tag+%25%7D%0A%7B%25+endunless+%25%7D%0A%60%60%60%0A%0A**Webhook+order+IDs%3A**+Use+%60order.admin_graphql_api_id%60+%28not+%60order.id%60%29+%E2%80%94+webhooks+give+REST+IDs%2C+you+need+the+full+GID.%0A%0A**userErrors%3A**+Every+mutation+needs+%60userErrors+%7B+field+message+%7D%60.%0A%0A**Liquid+syntax%3A**+All+tags+need+%60%7B%25+%25%7D%60+%E2%80%94+never+bare+%60else%60%2C+%60endif%60%2C+%60endfor%60.%0A%0A**Bulk+ops%3A**+Pass+query+with+%60%7B%7B+query+%7C+json+%7D%7D%60.+%60__typename%60+on+every+node.+%60__parentId%60+on+children.+Subscribe+to+%60mechanic%2Fshopify%2Fbulk_operation%60.+One+%60bulkOperationRunQuery%60+per+run.%0A%0A**Bulk+__typename%3A**+Use+CONCRETE+types%2C+never+interfaces.+%60AutomaticDiscountApplication%60+not+%60DiscountApplication%60.+Interface+names+match+zero+objects+in+JSONL.%0A%0A**Two-pass%3A**+Subscribe+to+%60mechanic%2Factions%2Fperform%60.+Pass+state+via+%60__meta%3A%60.+Results+at+%60action.run.result.data%60.+Use+%60__perform_event%3A+false%60+on+second-pass+actions.%0A%0A**Loop+prevention%3A**+On+update+events%2C+check+for+a+sentinel+tag+first.%0A%0A**Logging%3A**+Log+every+decision+point%2C+especially+skip+paths.%0A%0A**test_mode%3A**+Add+%60test_mode__boolean%60%3B+use+%60%7B%25+action+%22echo%22+%25%7D%60+instead+of+real+mutations.%0A%0AResources%3A+https%3A%2F%2Ftasks.mechanic.dev+%7C+https%3A%2F%2Flearn.mechanic.dev+%7C+https%3A%2F%2Fshopify.dev%2Fdocs%2Fapi%2Fadmin-graphql
```

You'll need to be logged in to a Shopify store. Copy the full URL above and paste it into your browser's address bar.

***

## What it provides

The skill teaches Sidekick the same patterns as the [Agent Skills](agent-skills.md) package:

* Complete importable JSON task output
* Async/sync pattern awareness (read with `| shopify`, write with `{% action %}`)
* Preview blocks for every subscribed event topic
* GraphQL-first Shopify API usage
* Proper webhook ID handling and tag comparison
* Bulk operation and two-pass workflow patterns

Sidekick will ask clarifying questions before writing — what should happen, what triggers it, and what conditions apply — then produce a complete task JSON you can import into Mechanic.

***

## Limitations

* **No live task library access.** Sidekick doesn't search [tasks.mechanic.dev](https://tasks.mechanic.dev/) in real time. For task discovery and reference, use the [MCP server](mcp.md) with an AI coding tool.
* **Session-based.** The skill is loaded via URL parameters and lasts for the current Sidekick session. Bookmark the link or return to this page to reload it.

***

## Related resources

* [Mechanic MCP Server](mcp.md) — connect your AI coding tool to the task library and docs
* [Mechanic Agent Skills](agent-skills.md) — install task-writing expertise into your AI coding tool
* [Using AI with Mechanic](../ai.md) — overview of all Mechanic AI tools
