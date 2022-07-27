---
title: 'Panta Rhei'
date: 2022-07-26T8:00:00+02:00
draft: true
summary: "Heraclitus was right. Πάντα ῥεῖ, everything flows, you can't step in the same river twice. That goes double if you're building a SaaS product..."
tags: ['seats.io', 'dev', 'startup']
---

I spend most of my professional time writing, deleting, reviewing and thinking about code. I do this at seats.io, a small company that I co-founded back in 2014. 

Seats.io does interactive floor plans for online ticketing websites. We currently serve around 250 customers, mostly online ticketing websites. Our customers integrate our seating charts into their website: they write code, both on the front- and the backend, using public API we provide and document. (TODO link) 

And typically, that's where the story ends: stuff works, and our customers expect stuff to keep working the way it does.  

At least, that's where it ends _for them_. Our API, like any other API, changes over the time when we fix bugs and build features. But we do want to keep everything running smoothly and not break any previous integrations.  

So the question arises: how can you keep things the same, while changing them continuously at the same time? 

In short, there are two solutions to this. Either you version your API, or you aim for backwards compatibility.

> Note that I'm specifically talking about SaaS products like seats.io here. Not libraries.    
For instance, at seats.io we have a number of API wrapper libraries (for php, java, .NET, etc) that make it easy to use the seats.io REST API in an idiomatic way. Those libs are versioned using semver (TODO link), as you would expect (though we tried not to (TODO link) at first, but that's a story for another day).

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

Maintaining backwards compatibility is a double-edged sword though: it means it becomes your responsibility to not _break_ existing integrations. And breaking changes can come in many flavours.  

* a breaking change in the technical sense. This is the most obvious form of a breaking change, e.g. changing or removing an API endpoint on which customers are relying. 
* a functionally breaking change, i.e. when you make other assumptions than your users, and you make a change with only the intended use case in mind. This is a gray area, of course.  
* shocking users. Your change does not _technically_ break anything, but you change the behaviour in such a way that users are   
* breaking internal/non-documented API. Sometimes integrators discover fields or methods they're not supposed to use, and use them anyway. If you then change those, you break their integration.   
* Fixing a bug on which integrators depend. It happens. 

In the following, I'll go over some strategies we use at seats.io, and that seem to be working.   

### Add or modify, don't remove
If you can change without breaking, do that. 
Can't do that without testing!

### Know your users. Know your product

### Don't lie & Don't be scared
On top of that, you will make mistakes. You will take product decisions that eventually will turn out to be based on wrong assumptions. You will introduce vocabulary that no-one understands or worse: that means something else to the rest of the world. You will

You will make mistakes. Explain to users that sometimes things don't work as expected, because they made sense in the past but not in the present anymore. 

### Break things!
You will also break things, eg when people discover & use internal API. That's great 👍!  

### Feature Flags FTW
Shield what cannot be fixed. 

#@# Sometimes you just have to break backwards compatibilty
Just once, years ago. And it took years too. 

# A final note


