# Mechanic-flavored Liquid

Everything in Mechanic is about [**tasks**](../../core-concepts/tasks/). Our tasks are written in **Liquid** – the same template language that Shopify uses for their online store themes, and for many other purposes.

Fundamentally, [**task code**](../../core-concepts/tasks/code/) is a Liquid template for rendering JSON instructions for [**actions**](../../core-concepts/actions/). In the same way that a Liquid theme might take Shopify store data and render HTML for a web browser, Mechanic task code takes incoming [**events**](../../core-concepts/events/), and uses them as variables to render JSON objects, defining work to be performed.

## Additions to the language

Task code only ever renders JSON objects. To make it easier to generate JSON, we've introduced several new Liquid tags: [action](tags/action.md), [log](tags/log.md), and [error](tags/error.md).

Mechanic has first-class support for [arrays](keyword-literals/array.md) and [hashes](keyword-literals/hash.md), via new keyword literals that make it easy to instantiate them. We've also enhanced the [assign](https://docs.usemechanic.com/article/357-the-assign-tag) tag to support assigning values within arrays and hashes.

Finally, we've added several [filters](../basics/filters.md) that make it easier to manage data in the Mechanic world. Highlights include [match](../filters.md#match) \(for regular expressions\), and [csv](../filters.md#csv), [base64](../filters.md#base-64-decode_base64), and [decode\_json](../filters.md#json-parse_json-parse_jsonl).

