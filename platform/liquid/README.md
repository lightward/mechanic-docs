# Liquid

Everything in Mechanic is about [**tasks**](../../core/tasks/), and tasks are written in **Liquid** – the same template language that Shopify uses for their online store themes, and for many other purposes.

Fundamentally, [**task code**](../../core/tasks/code/) is a Liquid template for rendering JSON instructions for [**actions**](../../core/actions/). In the same way that a Liquid theme might take Shopify store data and render HTML for a web browser, Mechanic task code takes incoming [**events**](../../core/events/), and uses them as variables to render JSON objects, defining work to be performed.

## Liquid basics

Mechanic's implementation of Liquid is based on the same engine that Shopify uses, which means that Mechanic shares the same basic Liquid features as Shopify does.

{% page-ref page="basics/" %}

## Mechanic's additions

Task code only ever renders JSON objects. To make it easier to generate JSON, we've introduced several new Liquid tags: [action](tags/action.md), [log](tags/log.md), and [error](tags/error.md).

{% page-ref page="tags/" %}

Mechanic has first-class support for [arrays](keyword-literals/array.md) and [hashes](keyword-literals/hash.md), via new keyword literals that make it easy to instantiate them. We've also enhanced the [assign](https://docs.usemechanic.com/article/357-the-assign-tag) tag to support assigning values within arrays and hashes.

{% page-ref page="keyword-literals/" %}

We've added several filters that make it easier to manage data in the Mechanic world. Highlights include [match](filters.md#match) \(for regular expressions\), and [csv](filters.md#csv), [base64](filters.md#base-64-decode_base64), and [decode\_json](filters.md#json-parse_json-parse_jsonl).

{% page-ref page="basics/filters.md" %}

Finally, Mechanic task code frequently interacts with special variables that Mechanic creates, representing specific types of data. These variables are examples of Mechanic-powered Liquid objects, which contain more intelligence than the usual Liquid variable.

{% page-ref page="objects/" %}

