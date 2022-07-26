---
title: 'Panta Rhei'
date: 2022-07-26T8:00:00+02:00
draft: true
summary: TODO
tags: ['seats.io', 'dev', 'startup']
---

I spend most of my professional time writing, deleting, reviewing and thinking about code. I do this at seats.io, a small company that I co-founded.  and that does interactive floor plans for online ticketing websites. We currently serve around 250 customers, mostly online ticketing websites, and we've been around since 2014. 

Our customers integrate our product into their website: they write code, both on the front- and the backend, using public API we provide and document. 

And typically, that's where the story ends: stuff works, and our customers expect stuff to keep working the way it does.  

At least, that's where it ends _for them_. Our API, like any other API, changes over the time when we fix bugs and build features. But we do want to keep everything running smoothly and not break any previous integrations.  

So Heraclitus was right: Πάντα ῥεῖ. Everything flows, and you can't step in the same river twice. 

But then the question arises: how do you keep things the same, while changing them all the time?

In short, there are two solutions to this. Either you version your API, or you aim for backwards compatibility.

# Versioning: no thanks
TODO rewrite
In a _startup_ - ugh, I hate that word -, especially in the early days, you can't afford the luxury of spending months on a pre-project, mapping out the necessary steps and requirements to start the initiation and subsequent delivery stages, planning, directing and managing deliverables. After all, you're not running a **PR**oject in a **C**ontrolled **E**nvironment (2?).  

Of course, I'm slightly exaggerating, but the point stands: things need to move, and they need to move fast.

TODO rewrite
You need to build something small, test it with a customer or two, then grow it, organically and in small steps. Change, see what works (and what does not!), and iteratively improve based on what you learned. The smaller the iterations, the better.

And that's the problem with versioning your API.  Atrocities like SemVer are not the way to go.

# Backwards Compatibility then?
Well, that's the route we took at seats.io. 

I'll go over some strategies that we use at seats.io, and that seem to be working.   

# Make things better
If you can change without breaking, do that. 
Can't do that without testing!

# Don't lie & Don't be scared
On top of that, you will make mistakes. You will take product decisions that eventually will turn out to be based on wrong assumptions. You will introduce vocabulary that no-one understands or worse: that means something else to the rest of the world. You will

You will make mistakes. Explain to users that sometimes things don't work as expected, because they made sense in the past but not in the present anymore. 

# Break things!
You will also break things, eg when people discover & use internal API. That's great 👍!  

# Feature Flags FTW
Shield what cannot be fixed. 

# Sometimes you just have to
Just once, years ago. And it took years too. 


