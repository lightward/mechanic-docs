# Trigger a Task From a Contact Form

## Tasks and Webhooks and Contact Forms, Oh My!

This tutorial will walk you through the process of setting up a custom task in Mechanic, which is called on Contact Form submission on your Shopify store, the contents of the form are passed to the task and which emails the contents of the contact form in CSV format.

### Requirements

* A Shopify store
* The Mechanic app installed. You can install it [here](https://apps.shopify.com/mechanic?ref=lightward)
* Basic knowledge of Liquid. [Need a refresher?](../../liquid/basics/)
* Basic knowledge of JavaScript. [Need a refresher?](https://www.w3schools.com/js/default.asp)

### The Situation

We have an online store called Mario's Mushrooms hosted on Shopify. Business is booming, and our mushrooms are being shipped all over the world. Our CEO, Mario, asks us to connect our Shopify Contact Form to our legacy Custom Relationshipment Management \(CRM\) system. We are eager to help, but the CRM doesn't have an API for us to integrate with. However, it can receive an email with a CSV attachment, which it will then import into its database. Perfect, we now have a path forward!

### The Plan

We are going to make a task in this cool Shopify app called Mechanic.  We'll code in Liquid! This task's job will be to take the contents of the contact form, convert it to CSV format, and attach it to an email, and send the mail to our CRM system. 

Once we have our task created, we need to figure out how to get our contact form to call our Mechanic task and provide the task with the contents of the contact form.

### The Mechanic Task

#### Our requirements

* Respond to contact form submissions from our shop's frontend.
* Take the submitted data and convert it to a CSV.
* Email the CSV to a given email address.

#### Step 1: Create a new task and subscribe to a custom event topic

![](../../.gitbook/assets/image%20%2812%29.png)

{% hint style="info" %}
Webhooks should be named after the service that will be sending you data, with an event topic that makes sense, using the format`user/subject/verb`

In this instance, we chose`user/webhook/form`
{% endhint %}

#### Step 2: Create a Mechanic webhook that points to our custom event topic

Start by opening Mechanic, from the "Apps" section of Shopify. Once in Mechanic, scroll down to the "Your account" section, and click the "Manage settings" link.

![](../../.gitbook/assets/image%20%2810%29.png)

Next, click the "+ Add a webhook" button, towards the end of the settings page.

![](../../.gitbook/assets/image%20%2813%29.png)

We give our webhook a name of Contact Form, we point it at our custom event topic`user/webhook/form`and click save, and a webhook URL is generated. Make note of the webhook URL so that we can use in the next steps.

![](../../.gitbook/assets/image%20%289%29.png)

The webhook URL will look something like this:

```
https://usemechanic.com/webhook/00000000-0000-0000-0000-000000000000
```

#### Step 3: Wire up the shop front-end to post data to our webhook

We have options here, the only requirement is that we POST to data to our webhook.  This can be done using pure JavaScript, jQuery, or even the action attribute of a form tag.  With other tools this would mean you need to start editing your theme, but with Mechanic we have the ability to add JavaScript to the online store front directly in the task editor. Mechanic leverages Shopify's [ScriptTag](https://shopify.dev/docs/admin-api/rest/reference/online-store/scripttag) API.  
  
For this tutorial, I created a development store and installed the [Debut theme](https://themes.shopify.com/themes/debut/styles/default). Next, I use the contact form that comes with the theme as the form that submits to our webook.  


![Note the id of this form is \#ContactForm](../../.gitbook/assets/image%20%2811%29.png)

This form has an HTML id of \#ContactForm. We are going to add an `EventListener` to listen for the `submit` event of this form. When the form is submitted, the data will first be posted to our webhook \(kicking off our Mechanic task\) and then the form will continue to submit like normal.

In your Mechanic task scroll down and click the "JavaScript for online storefront" link.

![](../../.gitbook/assets/image%20%2814%29.png)

Next, you'll see the code editor open where we can code the JavaScript that will be inserted in our shop frontend.

![](../../.gitbook/assets/image%20%288%29.png)

Copy and paste the JavaScript below into the JavaScript for online storefront.  Read the comments below for an explanation of the code.

{% tabs %}
{% tab title="JavaScript for online storefront" %}
```javascript
// This code gets inserted on all pages of your store
// So we need to ensure the ContactForm exists before we reference it below
// or we'll end up with an undefined error on some pages

const contactForm = document.querySelector('#ContactForm');

// if the contact form exists on the page that the script loaded
if(contactForm) {
  // listen for submit events for contact form
  contactForm.addEventListener("submit",function(e){
  // fetch is used to make POST request
  // https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
  // we take our webhook url as a task option 
  fetch({{ options.mechanic_webhook_url__required | json }}, {
    method: 'POST', 
    body: new FormData(contactForm),
  })
  .then(response => response.json())
  .then(data => {
    console.log('Sending data to Mechanic: Success!', data);
  })
  .catch((error) => {
    console.error('Sending data to Mechanic: Error!', error);
  });  
   // return true, which allows the regular form submit to continue
   return true;
  },false);
}

```
{% endtab %}
{% endtabs %}

#### Step 4: Capture our form submission on the Mechanic side, convert it to a CSV, and email it

When we submit the contact form from our Shop frontend the event data looks like this:

![](../../.gitbook/assets/image%20%2815%29.png)

The data we are interested in the contact object in the event data.

The first step is to extract this data and get it into a format that we can pass to the`csv`filter. Which takes a two-dimensional array and converts it to a CSV-formatted string.

{% tabs %}
{% tab title="Task Code" %}
```csharp
{% assign rows = array %}
{% assign header = array %}
{% assign header[0] = "Name" %}
{% assign header[1] = "email" %}
{% assign header[2] = "Phone Number" %}
{% assign header[3] = "Message" %}
{% assign rows[0] = header %}

{% assign row = array %}
{% assign row[0] = event.data.contact.Name %}
{% assign row[1] = event.data.contact.email %}
{% assign row[2] = event.data.contact["Phone Number"] %}
{% assign row[3] = event.data.contact.Message %}
{% assign rows[rows.size] = row %}
```
{% endtab %}
{% endtabs %}

The next step is to convert this array to a CSV and attach it to an email and sent it off.

{% tabs %}
{% tab title="Task Code" %}
```csharp
{% action "email" %}
  {
    "to": {{ options.recipient_email_address__email_required | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body__required_multiline | strip | newline_to_br | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }},
    "attachments": {
       {{ options.csv_attachment_filename__required | replace: ".csv", "" | append: ".csv" | json }}: {{ rows | csv | json }}
     }
   }
{% endaction %}
```
{% endtab %}
{% endtabs %}

As responsible developers, we also want to ensure we have a functioning event preview. At the top of our task code, we add in the stub data for the`event.preview`case

{% tabs %}
{% tab title="Task Code" %}
```csharp
{% if event.preview %}
  {% capture event_data_json %}
    {
      "contact": {
        "Name": "Matt Jake",
        "email": "email@email.com",
        "Phone Number": "555-555-5555",
        "Message": "Walking through a dream I see you"
       }
    }
  {% endcapture %}

  {% assign event = hash %}
  {% assign event["data"] = event_data_json | parse_json %}
{% endif %}
```
{% endtab %}
{% endtabs %}

### The Final Result

{% tabs %}
{% tab title="Task Code" %}
```csharp
{% if event.preview %}
  {% capture event_data_json %}
    {
      "contact": {
        "Name": "Matt Jake",
        "email": "email@email.com",
        "Phone Number": "555-555-5555",
        "Message": "Walking through a dream I see you"
       }
    }
  {% endcapture %}

  {% assign event = hash %}
  {% assign event["data"] = event_data_json | parse_json %}
{% endif %}
    
    
{% assign rows = array %}
{% assign header = array %}
{% assign header[0] = "Name" %}
{% assign header[1] = "email" %}
{% assign header[2] = "Phone Number" %}
{% assign header[3] = "Message" %}
{% assign rows[0] = header %}

{% assign row = array %}
{% assign row[0] = event.data.contact.Name %}
{% assign row[1] = event.data.contact.email %}
{% assign row[2] = event.data.contact["Phone Number"] %}
{% assign row[3] = event.data.contact.Message %}
{% assign rows[rows.size] = row %}

{% action "email" %}
  {
    "to": {{ options.recipient_email_address__email_required | json }},
    "subject": {{ options.email_subject__required | json }},
    "body": {{ options.email_body__required_multiline | strip | newline_to_br | json }},
    "reply_to": {{ shop.customer_email | json }},
    "from_display_name": {{ shop.name | json }},
    "attachments": {
       {{ options.csv_attachment_filename__required | replace: ".csv", "" | append: ".csv" | json }}: {{ rows | csv | json }}
     }
   }
{% endaction %}
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/image%20%2816%29.png)

