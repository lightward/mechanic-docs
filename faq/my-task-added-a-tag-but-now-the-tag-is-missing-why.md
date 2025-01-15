# My task added a tag, but now the tag is missing – why?

In most cases, this comes down to someone else – or som&#x65;_&#x74;hing_ else, like another app – working on the tagged resource simultaneously. Mechanic tasks are more or less additional apps, so also keep an eye out for potential conflicts in your own task library.

### Confirming where the tag went

For this, you'll need to contact Shopify support. (Mechanic can't help with this, and neither can the Mechanic team.) Shopify keeps logs for when tags were added and removed, and by whom – so, send them a message, referencing the related resource, the tag(s) that went missing, and when. They should be able to tell you who (or what) was responsible for the tag removal.

### Types of conflicts

#### Conflicts with a human user

Here's an example of how this sort of conflict could come about, using a hypothetical order (although the same flow would apply to products, customers, etc):

1. An order is created by a staff member, using the draft orders section of Shopify
2. Mechanic notices the new order, and adds a tag
3. The staff member still has the order screen open, which reflects _no_ tag, since they haven't refreshed
4. The staff member modifies the order in some way, and hits save
5. Shopify receives the order update, which includes an empty list of tags – this empty list overwrites Mechanic's tagging work, and the order now has no tags

The only solutions to this are human solutions. :) Make sure your staff is aware of the potential for conflicts of this sort.

#### Conflicts with another app

Here's how these conflicts tend to happen, using a product as an example:

1. A product is created
2. Mechanic notices the new product, and adds a tag
3. _Another_ app also notices the new product, and uses an outdated Shopify API (i.e. the REST admin API) to update its tags – this outdated API overwrites Mechanic's work, and Mechanic's new tag is lost

If you run into this scenario, the right thing to do is ask the app developer to move their app's tagging behavior over to Shopify's GraphQL admin API. This API allows tags to be added (or removed) one at a time, instead of sending along entire (possibly empty) tag lists which overwrite the tagging work of anyone else. (We talk more about this subject here, with specific regard to this API choice for Mechanic tasks: see [Tagging Shopify resources](../techniques/tagging-shopify-resources.md).)

#### Conflicts with another Mechanic task

If you're using tasks from Mechanic's library, this should only happen as a result of misconfiguration. If you've written your own tasks, do make sure that you're using GraphQL for managing Shopify tags (see [Tagging Shopify resources](../techniques/tagging-shopify-resources.md) for more on that subject).
