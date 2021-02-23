# Basics

If you have worked with Shopify themes, you've worked with [**Liquid**](https://shopify.github.io/liquid/), a template language created by Shopify.

Mechanic inherits all of Liquid's features, and adds some of its own. For more on Mechanic-only Liquid features, see [Mechanic Liquid](../mechanic-liquid/).

If you are new to Liquid or need a refresher, the next few sections will cover the basics of the language.

## Recognizing Liquid syntax

When reading Liquid code, you will notice two types of tags:

* `{{ order }}` – When code is wrapped in double curly braces, like this, the code inside the braces generate output.
* `{% assign is_mechanic_awesome = true %}` – When code is wrapped in single curly braces with percentage signs, like this, the code inside identifies logic and control flow. 

## Topics Covered

* [Comments](comments.md)
* [Variables](variables/)
* [Types](variables/types.md)
* [Comparisons](operators.md)
* [Conditionals](control-flow/)
* [Iteration](control-flow/iteration.md)
* [Filters](../basic-liquid-filters.md)
* [Whitespace](whitespace.md)

## Shopify Liquid cheat sheet

Shopify has created an awesome [cheat sheet for Liquid developers.](https://www.shopify.com/partners/shopify-cheat-sheet) The coverage of Liquid basics, tags, and filters is excellent, the only caveat is that the Liquid objects won't match up to the objects in Mechanic Liquid. The moral of the story is to use this cheat sheet, ingore the objects section unless you're developing Shopify themes, and you can learn more about the difference between Shopify Theme Liquid objects and Mechanic objects here: [Why aren't my Liquid variables giving me the Shopify data I expect?](https://docs.usemechanic.com/article/477-why-arent-my-liquid-variables-giving-me-the-shopify-data-i-expect)

## Shopify's introduction to Liquid

{% embed url="https://www.youtube.com/watch?v=tZLTExLukSg&feature=youtu.be" %}

{% hint style="info" %}
This content was compiled from the following Shopify resources.  If you want to dig deeper, make sure to visit them:

1. [https://shopify.github.io/liquid/](https://shopify.github.io/liquid/)
2. [https://shopify.dev/docs/themes/liquid/reference/basics](https://shopify.dev/docs/themes/liquid/reference/basics)
{% endhint %}

