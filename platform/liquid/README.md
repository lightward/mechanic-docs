# Liquid

Everything in Mechanic is about [**tasks**](../../core/tasks/), and tasks are written in **Liquid** – the same template language that Shopify uses for their online store themes, and for many other purposes.

Fundamentally, [**task code**](../../core/tasks/code/) is a Liquid template for rendering JSON instructions for [**actions**](../../core/actions/). In the same way that a Liquid theme might take Shopify store data and render HTML for a web browser, Mechanic task code takes incoming [**events**](../../core/events/), and uses them as variables to render JSON objects, defining work to be performed.

## One Liquid, many implementations

**Open-source Liquid** is a template language created by Shopify, used by many developers for many projects and products in, around, and beyond Shopify. ([Learn the basics here!](basics/))

**Shopify Liquid** is the implementation of open-source Liquid used within Shopify itself, including Shopify's Online Store themes. Shopify Liquid contains many filters and objects that are not a part of open-source Liquid.

**Mechanic Liquid** is the implementation of open-source Liquid used here, within Mechanic. It contains many filters and objects that are unique to Mechanic, in addition to some unique language features.

* [**Mechanic filters**](filters/) make it easier to manage data in the context of Mechanic tasks. Mechanic Liquid also includes support for many Shopify Liquid filters!
* [**Mechanic keyword literals**](keyword-literals/) create support for [arrays](keyword-literals/array.md) and [hashes](keyword-literals/hash.md).
* [**Mechanic objects**](objects/) represent specific Mechanic and Shopify resources.
* [**Mechanic tags**](tags/) make it easier for task writers to render JSON objects. These tags include [action](tags/action.md), [log](tags/log.md), and [error](tags/error.md). We've also enhanced the [assign](tags/assign.md) tag to support assigning values within arrays and hashes.
