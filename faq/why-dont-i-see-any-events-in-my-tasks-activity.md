# Why don't I see any events in my task's activity?

{% hint style="info" %}
This FAQ is about event filtering. If you're working on Shopify events specifically, refer also to [A Shopify event is missing. Where is it?](a-shopify-event-is-missing-where-is-it.md).
{% endhint %}

There are two Mechanic account settings that filter task and event activity, and they're designed to keep your activity list clutter-free:

* Hide events that did nothing
* Hide task runs that did nothing

If you're not seeing all of the events you expect to see, try disabling "Hide events that did nothing" – see instructions below. If you're not seeing your task showing up where you expect, try disabling "Hide task runs that did nothing".

If your task still isn't running or being triggered when you expect, double-check the task's configuration: in most cases, the task simply didn't decide that any actions were appropriate for incoming events. To help understand why this may be happening, [add log tags](../platform/liquid/tags/log.md) to your task code, to record variables involved in your task's decisions.

If you're not sure what to check next, or what to look for in your task setup, let us know using that chat button in the corner. We'd be glad to help you. :)

### Updating event filters

Event visibility filters can be adjusted on the fly, for an individual list of recent activity. Here's an example, using the recent activity list on the Mechanic homepage:

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e42c7d3a7e9ae69bd7/5e28a1e3b2a6d.gif)

These settings are available anywhere you see an activity list.

You can also use your Mechanic account settings to set the default values for these settings. To find these, open the Mechanic home screen, scroll down to the "Your account" area, then click the "Manage settings" link.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e304286364bc9443ef/5e28a1e3488b5.png)
