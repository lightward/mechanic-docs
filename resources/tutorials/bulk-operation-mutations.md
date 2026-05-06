---
description: "Run Shopify bulk operation mutations from a Mechanic task using staged JSONL uploads."
---

# Running bulk operation mutations

Shopify's `bulkOperationRunMutation` runs one mutation many times, using a JSONL file where each line contains the variables for one mutation execution. Mechanic can monitor the resulting Shopify bulk operation and return the completed JSONL result to the same task using `mechanic/shopify/bulk_operation`.

{% hint style="warning" %}
The example task below creates two test products. Run it in a development store, or adjust the mutation before using it in production.
{% endhint %}

## How the flow works

A bulk operation mutation has one extra step that bulk operation queries do not have: Shopify needs the variables in a staged JSONL file before the bulk mutation can start.

In Mechanic, this usually uses action result events to move through the workflow:

1. A `shopify` action calls `stagedUploadsCreate` with `resource: BULK_MUTATION_VARIABLES`.
2. A `mechanic/actions/perform` run uploads the JSONL variables file using the [HTTP action](../../core/actions/http.md).
3. Another `mechanic/actions/perform` run calls `bulkOperationRunMutation`, using the staged upload path from the first step.
4. A final `mechanic/actions/perform` run confirms Shopify accepted the bulk mutation.
5. A `mechanic/shopify/bulk_operation` run receives the completed bulk operation and reads `bulkOperation.objects`.

## Permissions

This example declares `read_products` and `write_products` explicitly:

```liquid
{% permissions %}
  read_products
  write_products
{% endpermissions %}
```

Mechanic can often infer permissions from Shopify actions, but staged JSONL files are uploaded directly to Shopify and are not visible to Mechanic's permission scanner. Explicit permissions are safest for bulk mutations, especially for generic mutations like `tagsAdd(id: $id)` or `metafieldsSet(metafields: $metafields)`, where the target resource IDs only appear in the JSONL file.

## Importable task export

Copy this task export into Mechanic to test the full staged upload and bulk mutation flow.

```json
{
  "name": "Test Shopify bulk operation mutation",
  "docs": "Runs a full Shopify bulk operation mutation test: creates a staged JSONL upload, uploads mutation variables, starts bulkOperationRunMutation, and logs the completed bulk operation result.",
  "script": "{% permissions %}\n  read_products\n  write_products\n{% endpermissions %}\n\n{% if event.topic == \"mechanic/user/trigger\" %}\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"create_staged_upload\" %}\n\n  {% action \"shopify\", __meta: meta %}\n    mutation {\n      stagedUploadsCreate(input: [{\n        resource: BULK_MUTATION_VARIABLES\n        filename: \"mechanic-bulk-mutation-test.jsonl\"\n        mimeType: \"text/jsonl\"\n        httpMethod: POST\n      }]) {\n        stagedTargets {\n          url\n          parameters {\n            name\n            value\n          }\n        }\n        userErrors {\n          field\n          message\n        }\n      }\n    }\n  {% endaction %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"create_staged_upload\" %}\n  {% assign staged_result = action.run.result.data.stagedUploadsCreate %}\n\n  {% if staged_result.userErrors != empty %}\n    {% error staged_result.userErrors %}\n  {% endif %}\n\n  {% assign staged_target = staged_result.stagedTargets.first %}\n  {% assign upload_body = hash %}\n  {% assign staged_upload_path = nil %}\n\n  {% for parameter in staged_target.parameters %}\n    {% assign upload_body[parameter.name] = parameter.value %}\n\n    {% if parameter.name == \"key\" %}\n      {% assign staged_upload_path = parameter.value %}\n    {% endif %}\n  {% endfor %}\n\n  {% assign timestamp = \"now\" | date: \"%Y%m%d%H%M%S\" %}\n\n  {% capture variables_jsonl %}\n{\"product\":{\"title\":\"Mechanic bulk mutation test {{ timestamp }} A\"}}\n{\"product\":{\"title\":\"Mechanic bulk mutation test {{ timestamp }} B\"}}\n  {% endcapture %}\n\n  {% capture bulk_mutation %}\nmutation call($product: ProductCreateInput!) {\n  productCreate(product: $product) {\n    product {\n      id\n      title\n    }\n    userErrors {\n      field\n      message\n    }\n  }\n}\n  {% endcapture %}\n\n  {% assign files = hash %}\n  {% assign files[\"file\"] = variables_jsonl | strip %}\n\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"run_bulk_mutation\" %}\n  {% assign meta[\"staged_upload_path\"] = staged_upload_path %}\n  {% assign meta[\"bulk_mutation\"] = bulk_mutation | strip %}\n\n  {% action \"http\", method: \"post\", url: staged_target.url, body: upload_body, files: files, __meta: meta %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"run_bulk_mutation\" %}\n  {% if action.run.result.status < 200 or action.run.result.status >= 300 %}\n    {% error action.run.result %}\n  {% endif %}\n\n  {% assign meta = hash %}\n  {% assign meta[\"stage\"] = \"start_bulk_mutation\" %}\n\n  {% action \"shopify\", __meta: meta %}\n    mutation {\n      bulkOperationRunMutation(\n        mutation: {{ action.meta.bulk_mutation | json }}\n        stagedUploadPath: {{ action.meta.staged_upload_path | json }}\n      ) {\n        bulkOperation {\n          id\n          status\n          type\n        }\n        userErrors {\n          field\n          message\n        }\n      }\n    }\n  {% endaction %}\n\n{% elsif event.topic == \"mechanic/actions/perform\" and action.meta.stage == \"start_bulk_mutation\" %}\n  {% assign bulk_mutation_result = action.run.result.data.bulkOperationRunMutation %}\n\n  {% if bulk_mutation_result.userErrors != empty %}\n    {% error bulk_mutation_result.userErrors %}\n  {% endif %}\n\n  {% log bulk_operation_started: bulk_mutation_result.bulkOperation %}\n\n{% elsif event.topic == \"mechanic/shopify/bulk_operation\" %}\n  {% log bulk_operation_type: bulkOperation.type %}\n  {% log bulk_operation: bulkOperation %}\n\n  {% for object in bulkOperation.objects %}\n    {% log bulk_mutation_result: object %}\n  {% endfor %}\n{% endif %}\n",
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
  {% for object in bulkOperation.objects %}
    {% log bulk_mutation_result: object %}
  {% endfor %}
{% endif %}
```

## Related

* [Bulk operations](../../core/shopify/read/bulk-operations.md)
* [Shopify action](../../core/actions/shopify.md)
* [HTTP action](../../core/actions/http.md)
* [Responding to action results](../../techniques/responding-to-action-results.md)
* [Permissions](../../core/tasks/permissions.md)
