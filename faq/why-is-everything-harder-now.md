# Why is everything harder now?

## User

{% hint style="info" %}
The following user message comes from our community Slack workspace — join at [slack.mechanic.dev](https://slack.mechanic.dev).
{% endhint %}

> I keep coming back to this and I just feel the need to vent about it 😅. I think \[another user] really hit the nail on the head here. The continued increase in complexity from Shopify's decision to retire REST is a real burden. It's really unfortunate, because from the beginning, what made Mechanic so attractive to me was its core proposition as I see it:
>
> > As a Shopify developer, I am already familiar with liquid from having worked on themes. Mechanic exists, and allows me to create automation logic using the same liquid language I'm already familiar with. Just like in a theme template, I have access to most of the properties I need through basic liquid objects. I don't have to learn how to construct a query, I can just look at the list of available properties and type it in once I've found what I need.
>
> _The barrier to entry was so low when I got started_, I could do pretty much everything I needed with the built-in liquid objects. It was a couple years before I even _attempted_ to learn GraphQL, and at that point it was only because I had become curious about building some much more complex solutions. Honestly, I'm not sure I ever would have gotten this far with Mechanic if I had to learn GraphQL on day one!
>
> I'm sure y'all at Lightward have had all these discussions extensively on the inside, and I don't envy the decisions you've had to make to solve them. Thanks for your dedication in supporting all us developers who depend on this platform!
>
> Ok, rant over 🙂.&#x20;
>
> Oh! I forgot the the thought that originally inspired me to come back to this!This goes for the entire Shopify platform, not Mechanic specifically... I just can't get over the disconnect between Shopify's webhooks being represented as REST objects, while at the same time having to interact with the API via GraphQL. Everything is named differently, everything is structured differently.
>
> It feels like, what is even the point of including any data at all in Shopify's webhooks? We're going to have to query a GraphQL representation of the data anyways, so if we even bother using the data from the webhook then we're forced to constantly deal with two different representations of the same data. Make it make sense please :melting\_face:

## Isaac

{% hint style="info" %}
Isaac created Mechanic. He posted the following in the same Slack thread that the preceding user message was in, responding directly to ... everything.
{% endhint %}

> The barrier to entry was so low when I got started

this used to be an explicit priority of shopify’s. it’s… different these days. back then, mechanic was sort of solving [the last mile problem](https://en.wikipedia.org/wiki/Last_mile_\(transportation\)) in a system that was otherwise conceptually simple and approachable. shopify is much less conceptually simple and approachable these days, and that “last mile” that mechanic used to solve is itself materially different. and that shows.

as a platform-on-a-platform, I think Mechanic will continuously evolve towards the simplest+cleanest possible treatment of Shopify’s API patterns. Shopify’s API patterns have exploded in complexity, and … honestly not only are we _unable_ to insulate the Mechanic user experience from that, I think it would be incorrect to aim for that end. Mechanic development is not separate from Shopify development. if Shopify’s developer experience is fundamentally changing (and it has/is), it’d be _incorrect_ for us to prevent the color and tone and rhythm of those changes from reaching the Mechanic developer experience.

nobody’s saying we should aim for that, of course. I’m pointing out the extreme end of the spectrum (where we-the-Mechanic-platform 100% insulates our developers from Shopify’s API surface evolution) to sort of sketch out the spectrum. on one end, there’s just raw-dogging the Shopify API without something Mechanic-like. on the other end, there’s Mechanic perfectly insulating you from the evolving complexity of the Shopify API. we are always somewhere in the middle.

_how_ we exist in the middle, the steps we choose in our dance there, that’s something that gets actively weighed every day

which is makes _community_ such a critical throughline of the entire Mechanic dealio. Matt and I don’t stand a chance at understanding the total dx (developer experience) without y’all. we’re all kind of collectively acting as senses for each other. the experience-sharing that happens here in slack (and in email support, and elsewhere) is what enables us to _begin_ to consider which design decisions are actually solid, viable, sustainable, and … I don’t know, _good_.

Shopify’s not going to keep this pace-of-change up forever. they’re sort of … well, they’re changing. the “arm the rebels” vibe isn’t _gone_, but it’s been joined by other priorities, and the ideological crystal of Shopify itself is stabilizing, finding a new \~stable configuration. it treats light differently. and we’re adapting. which is what we do. :) we do whatever it means to create a \~stable surface where it’s a little bit easier to keep one’s balance than on Shopify’s surface alone.

nb: I don’t aim to conclusively satisfy/soothe with my notes here. I aim to sketch out the state of play as I see it. satisfying and soothing are active processes, not states that are achieved and put to bed. Mechanic is a process. Lightward Inc is a process. hell, _Shopify_ is a process. this is me sharing how I see all of that. please keep the raw experience coming - \[users who've shared,] I’m incredibly grateful for the accounts you’ve shared here, in this thread and elsewhere
