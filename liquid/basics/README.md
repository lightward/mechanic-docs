# Basics

If you have worked with Shopify themes, you've worked with Liquid. If you are new to Liquid or need a refresher the next few sections will cover the basics of the language. 

### Liquid Syntax

When reading Liquid code you will notice curly braces and curl braces with percentages. The double curly braces `{{ order }}`,  identify output, and the single curly braces with percentages  `{% assign is_mechanic_awesome = true %}` , identify logic and control flow.  

### Topics Covered

* [Comments](comments.md)
* [Variables](variables.md)
* [Types](types.md)
* [Comparisons](comparison-operators.md)
* [Conditionals](conditional-statements.md)
* [Iteration](iteration.md)
* [Filters](basic-liquid-filters.md)
* [Whitespace](whitespace.md)

In later sections, we cover our additions to the Liquid language. To make our lives more comfortable here, we've introduced several new Liquid tags: [action](https://docs.usemechanic.com/article/359-the-action-tag), [log](https://docs.usemechanic.com/article/320-the-log-tag), and [error](https://docs.usemechanic.com/article/319-the-error-tag). We've also enhanced [the assign tag](https://docs.usemechanic.com/article/357-the-assign-tag) to support creating arrays and hashes.

### Shopify Liquid Cheatsheet

Shopify has created an awesome [cheat sheet for Liquid developers.](https://www.shopify.com/partners/shopify-cheat-sheet) The coverage of Liquid basics, tags, and filters is excellent, the only caveat is that the Liquid objects won't match up to the objects in Mechanic Liquid. The moral of the story is to use this cheat sheet, ingore the objects section unless you're developing Shopify themes, and you can learn more about the difference between Shopify Theme Liquid objects and Mechanic objects here: [Why aren't my Liquid variables giving me the Shopify data I expect?](https://docs.usemechanic.com/article/477-why-arent-my-liquid-variables-giving-me-the-shopify-data-i-expect)

### Introduction to Liquid Video

{% embed url="https://www.youtube.com/watch?v=tZLTExLukSg&feature=youtu.be" %}

{% hint style="info" %}
This content was compiled from the following Shopify resources.  If you want to dig deeper, make sure to visit them:

1. [https://shopify.github.io/liquid/](https://shopify.github.io/liquid/)
2. [https://shopify.dev/docs/themes/liquid/reference/basics](https://shopify.dev/docs/themes/liquid/reference/basics)
{% endhint %}



