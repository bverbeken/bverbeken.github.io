---
title: 'Panta Rhei'
date: 2022-07-26T8:00:00+02:00
draft: true
summary: "Heraclitus was right. Πάντα ῥεῖ, everything flows, you can't step in the same river twice. When building a SaaS product, that goes double: how do you change and evolve, while at the same time not change too much for existing users?"
tags: ['seats.io', 'dev', 'startup']
---

I spend most of my professional time writing, deleting, reviewing and thinking about code. I do this at seats.io, a small company that I co-founded back in 2014. 

Seats.io does interactive floor plans for online ticketing websites. We currently serve around 250 customers, mostly online ticketing websites. Our customers integrate our seating charts into their website: they write code, both on the front- and the backend, using public API we provide and document. (TODO link) 

And typically, that's where the story ends: stuff works, and our customers expect stuff to keep working the way it does, at least for a while.  

At least, that's where it ends _for them_. Most integrators, as I'll call them in this post, only review their integration when they do an overhaul of their software, or when they run into issues and discover. 

Our API however, like any other API, changes over time at a much smaller granularity. We continuously fix bugs and build new features. Which means we need to make sure to keep everything running smoothly and not break any existing integrations.  

So the question arises: how can you keep things the same, while changing them continuously at the same time? How do you make sure your customers at least have the impression they are stepping in the same river twice?  

In short, there are two solutions to this. 
1. version your API. Let users choose their version. You will have to support multiple versions. 
2. Everyone runs against the same version. Keep everything backwards compatible. 

> Note that, in the rest of this post, I'm specifically talking about SaaS products with a multi-tenant API, like seats.io. And so not about libraries!


# Backwards compatibility is your only option...
... unless you can afford the luxury of spending months on a pre-project, mapping out the necessary steps and requirements to start the initiation and subsequent delivery stages, planning, directing and managing deliverables. In that case and by all means: use PRINCE2 (TODO link). &lt;/sarcasm&gt;

However, in a _startup_ (ugh, I hate that word), you need to move, and you need to move _fast_. You need to build something small, test it with a customer or two, then grow it, organically and in small steps. Change, see what works (and what does not!), and iteratively improve based on what you learned. The smaller the iterations, the better.   

And that's the problem with versioning your API. Atrocities like SemVer are not the way to go. (TODO rewrite)

# Backwards Compatibility then?
Well, that's the route we took at seats.io at least. 

We only run one version in production at any given time, which has a number of advantages. 
* We don't have to decide or even think about whether something is a major, minor or bugfix release. This does not absolve us from thinking about consequences, though. See more below.  
* We can deploy whenever we want. Up to multiple times per day.  
* Users don't have to worry about upgrading. They get all the good stuff right away without doing anything!     

Maintaining backwards compatibility means you shift the responsibility of not breaking stuff to yourself. 

Breaking changes can come in many flavours:
* The most obvious form of a breaking change is a technical one. E.g. you change or remove an API endpoint on which integrators are relying. 
* You make a change to a feature with only the intended use case in mind. Except, integrators are making different assumptions, and are using the changed feature for something else than what you intended. In their eyes, you are breaking their use case. 
* Changing internal/non-documented API, on which you assume no-one is relying, but in fact they are. 
* shocking users. Your change does not _technically_ break anything, but you change the behaviour in such a way that users are
* Fixing a bug on which integrators depend. The line between a bug and a feature is not a line, it's a gradient.   

In the following, I'll go over some strategies we use at seats.io handle - and most of the time prevent - these situations.    

# Making a change: what are the options?

### If nobody is using a feature, change it at will 
When we have an idea on how to tackle a certain  
Some changes are 

If you can change without breaking, do that. 
Can't do that without testing!

### Add, don't modify or remove

### Warn, don't throw 

### Break it

### Feature Flags FTW
Shield what cannot be fixed.

### Sometimes you just have to break backwards compatibilty
Just once, years ago, we did deliberately break backwards compatibility. And it took us years to move everyone.
 

# Conditions

### BC should be a team reflex

### Know your users & know your product

### Be prepared for mistakes.
When you grow a product iteratively, you are bound to make mistakes. You will take product decisions that eventually will turn out to be based on wrong assumptions. You will introduce vocabulary that no-one understands. Or worse: vocabulary that has a different meaning altogether for your customers. 

If when that happens, explain to users that sometimes things don't work as expected, because they made sense in the past but not in the present anymore. 



# A final note


