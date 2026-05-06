---
description: "Run Shopify bulk operation mutations from a Mechanic task using staged JSONL uploads."
---

# Running bulk operation mutations

Shopify's `bulkOperationRunMutation` runs one mutation many times, using a JSONL file where each line contains the variables for one mutation execution. Mechanic can monitor the resulting Shopify bulk operation and return the completed JSONL result to the same task using `mechanic/shopify/bulk_operation`.

{% hint style="warning" %}
The example task below creates two test products. Run it in a development store, or adjust the mutation before using it in production.
{% endhint %}

## When to use this pattern

Use a bulk operation mutation when you need to run the same Shopify-supported and Mechanic-allowed mutation many times, and your task can wait for an asynchronous result. This is useful for high-volume imports or updates where normal one-by-one GraphQL actions would be slower or harder to throttle.

Do not use this pattern when each mutation depends on the result of the previous one, when the order of execution matters, or when your task needs the result immediately in the same run.

Shopify allows most Admin API mutations in `bulkOperationRunMutation`, except recursive bulk operation mutations such as `bulkOperationRunMutation` and `bulkOperationRunQuery` themselves. Shopify also limits the mutation string to one connection field. Mechanic still applies the Shopify action's mutation safety checks; if a mutation is not allowed in Mechanic, the action run will fail with an error.

{% hint style="info" %}
Mechanic's HTTP action uses file generators for multipart uploads, and generated files have a 20MB per-file limit. Shopify's bulk import JSONL limit may be higher than that, so split large imports into batches that fit Mechanic's generated-file limit.
{% endhint %}

## How the flow works

A bulk operation mutation has one extra step that bulk operation queries do not have: Shopify needs the variables in a staged JSONL file before the bulk mutation can start.

In Mechanic, this usually uses action result events to move through the workflow:

1. A `shopify` action calls `stagedUploadsCreate` with `resource: BULK_MUTATION_VARIABLES`.
2. A `mechanic/actions/perform` run uploads the JSONL variables file using the [HTTP action](../../core/actions/http.md).
3. Another `mechanic/actions/perform` run calls `bulkOperationRunMutation`, using the staged upload path from the first step.
4. A final `mechanic/actions/perform` run confirms Shopify accepted the bulk mutation.
5. A `mechanic/shopify/bulk_operation` run receives the completed bulk operation and reads `bulkOperation.objects`.

Shopify requires the bulk mutation to finish within 24 hours. If a job fails or gets too large, reduce the input size and run multiple batches.

## Permissions

This example declares `read_products` and `write_products` explicitly:

```liquid
{% permissions %}
  read_products
  write_products
{% endpermissions %}
```

Mechanic can often infer permissions from Shopify actions, but staged JSONL files are uploaded directly to Shopify and are not visible to Mechanic's permission scanner. Explicit permissions are safest for bulk mutations, especially for generic mutations like `tagsAdd(id: $id)` or `metafieldsSet(metafields: $metafields)`, where the target resource IDs only appear in the JSONL file.

## Try it now

Use the importable example below if you need to test the full staged upload and bulk mutation flow.

When writing your own task from a blank task, start typing `boilerplate:bulk_operation_mutation` in the code editor to insert starter code for this full pattern. Use `boilerplate:staged_upload_jsonl` if you only need the staged upload portion.

After importing it:

1. Run the task manually.
2. Watch the task move through three `mechanic/actions/perform` stages: `create_staged_upload`, `run_bulk_mutation`, and `start_bulk_mutation`.
3. Wait for Shopify to finish the bulk operation.
4. Open the later `mechanic/shopify/bulk_operation` run and inspect the logs for line-level results.

<details>

<summary>Importable task export</summary>

```json
{
  "name": "Test Shopify bulk operation mutation",
  "docs": "Runs a full Shopify bulk operation mutation test: creates two test products using a staged JSONL upload, starts bulkOperationRunMutation, and logs the completed line-level results. Use this in a development store, or adjust the mutation before using it in production.",
  "script": "{% permissions %}\n  read_products\n  write_products\n{% endpermissions %}\n\n{% if event.topic == \"mechanic/user/trigger\" %}\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"create_staged_upload\" %}\n\n  {% action \"shopify\", __meta: meta %}\n    mutation {\n      stagedUploadsCreate(input: [{\n        resource: BULK_MUTATION_VARIABLES\n        filename: \"mechanic-bulk-mutation-test.jsonl\"\n        mimeType: \"text/jsonl\"\n        httpMethod: POST\n      }]) {\n        stagedTargets {\n          url\n          parameters {\n            name\n            value\n          }\n        }\n        userErrors {\n          field\n          message\n        }\n      }\n    }\n  {% endaction %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"create_staged_upload\" %}\n  {% if action.run.ok == false %}\n    {% error action.run.error %}\n  {% endif %}\n\n  {% assign staged_result = action.run.result.data.stagedUploadsCreate %}\n\n  {% if staged_result.userErrors != empty %}\n    {% error staged_result.userErrors %}\n  {% endif %}\n\n  {% assign staged_target = staged_result.stagedTargets.first %}\n  {% assign upload_body = hash %}\n  {% assign staged_upload_path = nil %}\n\n  {% for parameter in staged_target.parameters %}\n    {% assign upload_body[parameter.name] = parameter.value %}\n\n    {% if parameter.name == \"key\" %}\n      {% assign staged_upload_path = parameter.value %}\n    {% endif %}\n  {% endfor %}\n\n  {% assign timestamp = \"now\" | date: \"%Y%m%d%H%M%S\" %}\n\n  {% capture variables_jsonl %}\n{\"product\":{\"title\":\"Mechanic bulk mutation test {{ timestamp }} A\"}}\n{\"product\":{\"title\":\"Mechanic bulk mutation test {{ timestamp }} B\"}}\n  {% endcapture %}\n\n  {% capture bulk_mutation %}\nmutation call($product: ProductCreateInput!) {\n  productCreate(product: $product) {\n    product {\n      id\n      title\n    }\n    userErrors {\n      field\n      message\n    }\n  }\n}\n  {% endcapture %}\n\n  {% assign files = hash %}\n  {% assign files[\"file\"] = variables_jsonl | strip %}\n\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"run_bulk_mutation\" %}\n  {% assign meta[\"staged_upload_path\"] = staged_upload_path %}\n  {% assign meta[\"bulk_mutation\"] = bulk_mutation | strip %}\n\n  {% action \"http\", method: \"post\", url: staged_target.url, body: upload_body, files: files, __meta: meta %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"run_bulk_mutation\" %}\n  {% if action.run.ok == false %}\n    {% error action.run.error %}\n  {% endif %}\n\n  {% if action.run.result.status < 200 or action.run.result.status >= 300 %}\n    {% error action.run.result %}\n  {% endif %}\n\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"start_bulk_mutation\" %}\n\n  {% action \"shopify\", __meta: meta %}\n    mutation {\n      bulkOperationRunMutation(\n        mutation: {{ action.meta.bulk_mutation | json }}\n        stagedUploadPath: {{ action.meta.staged_upload_path | json }}\n      ) {\n        bulkOperation {\n          id\n          status\n          type\n        }\n        userErrors {\n          field\n          message\n        }\n      }\n    }\n  {% endaction %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"start_bulk_mutation\" %}\n  {% if action.run.ok == false %}\n    {% error action.run.error %}\n  {% endif %}\n\n  {% assign bulk_mutation_result = action.run.result.data.bulkOperationRunMutation %}\n\n  {% if bulk_mutation_result.userErrors != empty %}\n    {% error bulk_mutation_result.userErrors %}\n  {% endif %}\n\n  {% log bulk_operation_started: bulk_mutation_result.bulkOperation %}\n\n{% elsif event.topic == \"mechanic/shopify/bulk_operation\" %}\n  {% log bulk_operation_type: bulkOperation.type %}\n  {% log bulk_operation: bulkOperation %}\n\n  {% if bulkOperation.status == \"FAILED\" %}\n    {% error bulkOperation %}\n  {% endif %}\n\n  {% for object in bulkOperation.objects %}\n    {% assign result = object.data.productCreate %}\n\n    {% if object.errors != blank %}\n      {% log bulk_mutation_graphql_errors: object %}\n    {% elsif result.userErrors != empty %}\n      {% log bulk_mutation_user_errors: object %}\n    {% else %}\n      {% log created_product: result.product %}\n    {% endif %}\n  {% endfor %}\n{% endif %}\n",
  "subscriptions": [
    "mechanic/user/trigger",
    "mechanic/actions/perform",
    "mechanic/shopify/bulk_operation"
  ],
  "subscriptions_template": "mechanic/user/trigger\nmechanic/actions/perform\nmechanic/shopify/bulk_operation",
  "options": {},
  "tags": [],
  "halt_action_run_sequence_on_error": false,
  "perform_action_runs_in_sequence": false,
  "online_store_javascript": null,
  "order_status_javascript": null,
  "preview_event_definitions": []
}
```

</details>

## The important pieces

The staged upload is created with Shopify's `BULK_MUTATION_VARIABLES` resource:

```graphql
mutation {
  stagedUploadsCreate(input: [{
    resource: BULK_MUTATION_VARIABLES
    filename: "mechanic-bulk-mutation-test.jsonl"
    mimeType: "text/jsonl"
    httpMethod: POST
  }]) {
    stagedTargets {
      url
      parameters {
        name
        value
      }
    }
    userErrors {
      field
      message
    }
  }
}
```

The JSONL file contains one variables object per line:

```json
{"product":{"title":"Mechanic bulk mutation test A"}}
{"product":{"title":"Mechanic bulk mutation test B"}}
```

Each top-level JSONL key must match a variable in the mutation string. In this example, each line has a `product` key because the mutation declares `$product`.

The HTTP action uploads that JSONL as a multipart file. The `files["file"]` key is intentional: Shopify staged uploads expect the uploaded file field to be named `file`. Do not manually set a multipart `Content-Type` header; Mechanic sets the multipart boundary for you.

```liquid
{% assign files = hash %}
{% assign files["file"] = variables_jsonl | strip %}

{% action "http", method: "post", url: staged_target.url, body: upload_body, files: files, __meta: meta %}
```

The `stagedUploadPath` value comes from the staged upload parameter named `key`.

```liquid
{% if parameter.name == "key" %}
  {% assign staged_upload_path = parameter.value %}
{% endif %}
```

After the upload succeeds, the task can start the bulk mutation:

```graphql
mutation {
  bulkOperationRunMutation(
    mutation: "mutation call($product: ProductCreateInput!) { productCreate(product: $product) { product { id title } userErrors { field message } } }"
    stagedUploadPath: "tmp/example/bulk_mutation_variables.jsonl"
  ) {
    bulkOperation {
      id
      status
      type
    }
    userErrors {
      field
      message
    }
  }
}
```

When Shopify completes the operation, Mechanic invokes the same task with `mechanic/shopify/bulk_operation`. Use `bulkOperation.type` to distinguish mutation results from query results:

```liquid
{% if bulkOperation.type == "MUTATION" %}
  {% if bulkOperation.status == "FAILED" %}
    {% error bulkOperation %}
  {% endif %}

  {% for object in bulkOperation.objects %}
    {% assign result = object.data.productCreate %}

    {% if object.errors != blank %}
      {% log bulk_mutation_graphql_errors: object %}
    {% elsif result.userErrors != empty %}
      {% log bulk_mutation_user_errors: object %}
    {% else %}
      {% log created_product: result.product %}
    {% endif %}
  {% endfor %}
{% endif %}
```

For bulk mutations, each result object is the GraphQL response for one JSONL input line. A completed bulk operation can still contain per-line `errors` or `userErrors`; check those fields before treating a line as successful. Shopify can include `__lineNumber` in the output to help connect a result back to the input line.

## Related

* [Bulk operations](../../core/shopify/bulk-operations.md)
* [Shopify action](../../core/actions/shopify.md)
* [HTTP action](../../core/actions/http.md)
* [Responding to action results](../../techniques/responding-to-action-results.md)
* [Permissions](../../core/tasks/permissions.md)
