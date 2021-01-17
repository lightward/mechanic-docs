# Trigger a Task From a Contact Form

## Tasks and Webhooks and Contact Forms, Oh My!

This tutorial will walk you through the process of setting up a custom task in Mechanic, which is called on Contact Form submission on your Shopify store, the contents of the form are passed to the task and which emails the contents of the form in CSV format.

### Requirements

* A Shopify store
* The Mechanic app installed. You can install it [here](https://apps.shopify.com/mechanic?ref=lightward)
* Basic knowledge of Liquid.[ Need a refresher?](../core-concepts/liquid.md)
* Basic knowledge of JavaScript

### The Situation

We have an online store called Mario's Mushrooms hosted on Shopify. Business is booming and our mushrooms are being shipped all over the world. Our CEO, Mario, asks us to connect our Shopify Contact Form to our legacy Custom Relationshipment Management \(CRM\) system. We are eager to help, but the CRM doesn't have an API for us to integrate with.  It can however recieve an email with a CSV attachment, which it will then import into its database. Perfect, we now have a path forward!

### The Plan

We are going to make a task in this cool Shopify app called Mechanic.  We'll code in Liquid! This task's job will be to take the contents of the contact form, convert it to CSV format, and attach it to an email, and send the mail to our CRM system. 

Once we have our task created, we need to figure out how to get our contact form to call our Mechanic task and provide the task with the contents of the contact form.

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}



