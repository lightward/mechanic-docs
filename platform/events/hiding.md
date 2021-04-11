# Hiding events

There are two Mechanic account settings that filter task and event activity, and they're designed to keep the activity list clutter-free:

* Hide events that did nothing
* Hide task runs that did nothing

If an expected event is not appearing, try disabling "Hide events that did nothing" – see instructions below. If a specific task isn't appearing in the activity list, and should be, try disabling "Hide task runs that did nothing".

If the task still isn't running when expected, double-check the task's configuration: in most cases, the task simply didn't decide that any actions were appropriate for the incoming events. To help understand why this may be happening, add [log tags](../liquid/tags/log.md) to the task code, recording variables involved in your task's decisions.

{% hint style="info" %}
These settings are unrelated to [event filters](filters.md). Hiding events does not change Mechanic's treatment of those events, whereas event filters actively prevent Mechanic from processing certain events.
{% endhint %}

## Usage

These event visibility filters can be adjusted on the fly, for an individual list of recent activity. Here's an example, using the recent activity list on the Mechanic homepage:

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e42c7d3a7e9ae69bd7/5e28a1e3b2a6d.gif)

These settings are available in every activity list, throughout Mechanic.

The defaults for these settings may be modified for each Mechanic account. To find these, open the Mechanic home screen, scroll down to the "Your account" area, then click the "Manage settings" link.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e28a1e304286364bc9443ef/5e28a1e3488b5.png)

