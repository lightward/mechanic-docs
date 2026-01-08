---
description: >-
  Analytics is a powerful reporting tool available to all merchants on Shopify.
  Mechanic can utilize the same ShopifyQL queries that generate those reports.
---

# Query analytics data with ShopifyQL

This tutorial will demonstrate the basic concepts on how to use [ShopifyQL](https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/shopifyql-editor) queries in Mechanic tasks to query Shopify analytics data.

{% hint style="info" %}
Review this tutorial in conjunction with the [Demonstration: Query analytics data with ShopifyQL](https://tasks.mechanic.dev/demonstration-query-analytics-data-with-shopifyql) task.
{% endhint %}

ShopifyQL queries in Mechanic work essentially the same as other [GraphQL queries](../../core/shopify/read/graphql-in-liquid.md), by capturing a GraphQL query and passing it to the Shopify Admin API using the [shopify](../../platform/liquid/filters/#shopify) Mechanic Liquid filter, and then immediately accessing the query results in the same task run.

A key difference is that there will be an additional query embedded within the [shopifyqlQuery](https://shopify.dev/docs/api/admin-graphql/latest/queries/shopifyqlQuery) query structure. This embedded query holds the [ShopifyQL syntax](https://shopify.dev/docs/api/shopifyql).

#### Basic structure of a ShopifyQL query task

```liquid
{%- capture shopifyql_query -%}
  FROM sales
  SHOW net_items_sold, gross_sales, discounts, returns, net_sales, taxes, total_sales
  WHERE product_vendor IS NOT NULL
  GROUP BY product_vendor
  DURING last_year
  ORDER BY total_sales DESC
  LIMIT 1000
{%- endcapture -%}

{% capture query %}
  {
    shopifyqlQuery(query: {{ shopifyql_query | json }}) {
      tableData {
        columns {
          displayName
          name
          dataType
          subType
        }
        rows
      }
      parseErrors
    }
  }
{% endcapture %}

{% assign result = query | shopify %}

{% assign rows = result.data.shopifyqlQuery.tableData.rows %}
{% assign columns = result.data.shopifyqlQuery.tableData.columns %}

{% log rows_count: rows.size %}
{% log rows: rows %}
{% log columns: columns %}
```

{% hint style="info" %}
Use another Liquid capture block for the ShopifyQL syntax, in lieu of trying to embed it directly within the outer GraphQL query. You may also use a multiline task options field to hold the ShopifyQL syntax.
{% endhint %}

{% hint style="info" %}
The `columns` field is mostly useful when developing your custom task, to see the field names and types returned by the query. The `rows` data returned in the query will contain the field names and data as key value pairs in each row object, so the columns field is technically not needed to work with the query results.
{% endhint %}

#### Example ShopifyQL query result (JSON)

{% code title="" %}
```json
{
  "data": {
    "shopifyqlQuery": {
      "tableData": {
        "columns": [
          {
            "displayName": "Product vendor",
            "name": "product_vendor",
            "dataType": "STRING",
            "subType": null
          },
          {
            "displayName": "Net items sold",
            "name": "net_items_sold",
            "dataType": "INTEGER",
            "subType": null
          },
          {
            "displayName": "Gross sales",
            "name": "gross_sales",
            "dataType": "MONEY",
            "subType": null
          },
          {
            "displayName": "Discounts",
            "name": "discounts",
            "dataType": "MONEY",
            "subType": null
          },
          {
            "displayName": "Returns",
            "name": "returns",
            "dataType": "MONEY",
            "subType": null
          },
          {
            "displayName": "Net sales",
            "name": "net_sales",
            "dataType": "MONEY",
            "subType": null
          },
          {
            "displayName": "Taxes",
            "name": "taxes",
            "dataType": "MONEY",
            "subType": null
          },
          {
            "displayName": "Total sales",
            "name": "total_sales",
            "dataType": "MONEY",
            "subType": null
          }
        ],
        "rows": [
          {
            "product_vendor": "ACME",
            "net_items_sold": "20",
            "gross_sales": "800.0",
            "discounts": "0",
            "returns": "-24.0",
            "net_sales": "776.00",
            "taxes": "7.76",
            "total_sales": "783.76"
          },
          {
            "product_vendor": "Company 123",
            "net_items_sold": "2",
            "gross_sales": "86.55",
            "discounts": "-30.25",
            "returns": "0",
            "net_sales": "56.3",
            "taxes": "5.63",
            "total_sales": "61.93"
          }
        ]
      }
    }
  }
}
```
{% endcode %}

The rows object will contain all of the data you need from the query. Running the task once during initial development to log out the rows result will give you the data field names you can work with.

From this point, the entire arsenal of features available in Mechanic can be used to do whatever you'd like with the data. Tag customers, orders, or products. Send data to [Airtable](../../platform/integrations/airtable.md), [Google Sheets](../../core/actions/integrations/google-sheets.md), or [Slack](../../core/actions/integrations/slack.md). Generate a CSV file and send via email or upload to an FTP server. Etc.

#### Some additional development tips

* Review the existing reports available in Analytics to get ideas on how you might use ShopifyQL queries in automations
* Use Sidekick (Shopify Analytics -> Reports -> New exploration) to help you generate and validate ShopifyQL syntax
* Clean up ShopifyQL queries for use by Mechanic:
  * Remove the VISUALIZE line (this has no effect for data retrieval)
  * When using a GROUP BY clause, generally you can safely remove the WITH TOTALS qualifier, as the summed totals across groups is often not useful
  * If expecting more than 1000 rows of data, then add a LIMIT line with a value greater than needed

