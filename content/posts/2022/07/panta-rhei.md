---
title: 'Panta Rhei'
date: 2022-07-29T8:00:00+02:00
summary: "Heraclitus said Πάντα ῥεῖ: everything flows, you can't step in the same river twice. That's true when you're building a SaaS product as well: how can you evolve your product, while at the same time keeping it the same for existing users?"
tags: ['seats.io', 'dev', 'startup', 'versioning', 'API']
---

I spend most of my professional time writing, deleting, reviewing and thinking about code. I do this at seats.io, a small company that I co-founded back in 2014. 

Seats.io does interactive floor plans for online ticketing websites. Our customers integrate our seating charts into their ticketing website or app: they write code, both on the front- and the backend, using API we [provide](https://docs.seats.io/docs/).   

And typically, that's where the story ends: stuff works, and our customers expect stuff to keep working the way it does.

At least, that's where it ends _for them_. Most users only review their integration when they do an overhaul of their software, or when they run into issues and discover. 

Our product however, like any software product, changes over time at a much smaller granularity. We continuously fix bugs and build new features. Which means we need to make sure to keep everything running smoothly and not break any existing integrations.  

So then the question arises: how do you keep things the same, while changing them continuously at the same time? How do you not break existing integrations, while continuously evolving your product? 

In short, there are two solutions to this. 
1. Version your API. Let users choose their version.  
2. Run a single version, which is used by everyone. And keep everything backwards compatible.

# Versioning our API? No thanks.
_(Note: in the following, I'm specifically talking about SaaS products with a multi-tenant API here. This post is not about software libraries!)_

In a startup (ugh, I hate that word) like ours, you need to move, and you need to move _fast_. You need to build something small, test it with a customer or two, then grow it, organically and in small steps. Change, see what works (and what does not!), and iteratively improve based on what you learned. The smaller the iterations, the better.

Now imagine creating, managing and running(!) a new version, every time you make a (potentially) breaking change. Before long, each one of your users is on a different version, each of which you need to maintain.  

You'll quickly end up in the realm of release calendars, planning and estimates. When all you want to do is push product out the door.

# Backwards Compatibility then?
Well, that's the route we took at seats.io. The rest of this post is about how we make that work. 

Running a single version in production at any given time has a number of major advantages:  
* You don't have to worry about version numbers. No discussions about whether something is a bug fix or a new feature.  
* There's no need for a release schedule or calendar, or even planning whatsoever. You're completely free to deploy new versions at will; though I would recommend to keep a changelog for past changes, of course. 
* Users don't need to worry about upgrading. They get all the good stuff right away without doing anything!     
 
The disadvantage though, is that you make the burden of not breaking stuff your own: you have to live up to the responsibility of not breaking your customer's code. 

That's not an easy feat, considering that breaking changes come in many flavours.     

* The most obvious form of a breaking change is a technical one. E.g. you change or remove an API endpoint on which integrators are relying. 
* You make a change to a feature with only its _intended_ use case in mind. In other words: you think this feature does X, but for some users it does Y as well. After your change, they cannot use the feature for their use case anymore, and so in their eyes, you are breaking their product. 
* Changing internal/non-documented API, on which you assume no-one is relying, but in fact they are 😱. 
* shocking users. Your change does not _technically_ break anything, but you change the behaviour or even looks in such a way that users are shocked and don't know what to do anymore. Usually happens with the less technically gifted, but that's not a hard rule. 
* Fixing a bug on which integrators depend. The line between a bug and a feature is not a line, it's a gradient.

# Changing tactics
So in short, any change can be a breaking change. And you have no way to surely predict whether a change will turn out to be a breaking one, because much of the breaking happens in the head or the code of your users. 

What _can_ you do, then? Well, here's a couple of tactics we use. 

1. **Add, don't modify or remove.**
If you can change without breaking anything, do that. Good examples are adding a field to a json object you expose on an API endpoint. Obviously, an extensive automated test suite helps here. 

2. **Warn, don't throw.** 
Sometimes, you can avoid actually breaking stuff by instead giving a warning, e.g. in the browsers console. For instance, we never just remove configuration options on our seating charts. Instead, we log a warning saying this option is now deprecated, and we measure the use. \
While this does result in sometimes long periods of time during which a feature is deprecated, at least you didn't break.    

3. **Feature Flags!**
Most of the time, if we cannot solve a problem in a 100% backwards compatible way, we create a feature flag: a configuration option that allows you to switch between the old and the new behaviour. You can then enable that feature flag for new signups, and keep it disabled for existing users - unless they ask to be switched. \
Yes, this comes at the price of extra complexity, but at least you're not breaking your customer's product. Nor their trust.

4. **Be comfortable breaking it**
Sometimes, when discussing a feature's backwards compatibility, we say things like "Oh God I hope nobody's doing [insert crazy idea here]. They wouldn't would they?" (they would). Or we didn't even think about the possibility that someone out there might be using that particular non-documented field and remove it.
If we then go ahead, make the change, and it turns out to actually break stuff for that customer (it's always just a single one in this case), we either roll back the deploy and give them some time to fix their integration, or propose a workaround.   

# It's all about the team
The tactics above will help, but not cover all the cases. So besides those, there are practices (ugh, another ugly word) we use to mitigate the chances of introducing breaking changes, or at least have a way to deal with them.  

1. **backwards compatibility should always be top of mind**
   Whenever we discuss features, bugs, support requests, anything product-related really, we always, always ask ourselves: can this solution break stuff, and how? It's a team reflex.

2. **Know your users, know your product**
   Seats.io offers a myriad of features (best available seat selection, bookable tables, boxes, sales channels, social distancing, ticket pricing and discounts, season tickets, etc etc), to over 250 customers globally who all use different combinations of those features, on different sizes and types of venues (stadiums, theatres, table layouts, multi-floor trade shows, etc), and for different reasons (ticketing, registration, checkin, etc). \
   If you need to maintain backwards compatibiility in such a context, you need to know who is using what and what for. Or at least be able to quickly answer that question for a given feature.

3. **Test, test, and then test some more.**
   Of course, you need an automated test suite you can trust and fall back on. The problem is that that's just part of the story. You can only write tests based on your own assumptions, and your customers might be making different assumptions. \
   That's why, everytime we deploy a new version, we first test live customer sites against the new version (e.g. using [Chrome Redirector](https://chrome.google.com/webstore/detail/redirector/ocgpenflpmgnfapjedencafcfakcekcd?hl=en)). Not only does that help detecting breaking changes, it's also a great way to get to know your customers and what they do with your product!

4. **Be aware that this takes trust, and that trust takes time**
   It's one thing to tell customers that they don't have to bother with upgrades. It's another to actually convince them they can trust you. And building trust takes time. \
   Which takes me to 👇

5. **Be open about mistakes**
   When you do break things, it will happen eventually, be upfront about what happened. Investigate, contact your users, let them know you understand this is important. And do your stinking best to fix it asap.

6. **Be prepared to roll back.**
   Deploy small changes, and deploy often. Don't deploy right before heading home to feed the kids. Have a "roll back deploy" button nearby. Test that button once in a while. You know the drill, those things are important.



# Final note
In this post, I made the point that versioning the API of your SaaS business is a bad idea, why we went for the backwards compatibility route, and listed some strategies we use in our team to make that route work for us. 

Again, I want to stress that this is about the API itself. There's more than meets the eye here: For instance, we do have API wrapper libraries (in Java, js, ruby, python and whatnot), which are _of course_ versioned using [SemVer](https://semver.org/). I might write a follow-up post on the difference between the two approaches.  


