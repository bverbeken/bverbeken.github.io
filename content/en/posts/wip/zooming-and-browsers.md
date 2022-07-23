---
title: 'TODO'
date: 2022-07-22T8:00:00+02:00
draft: true
summary: TODO
tags: ['devlife']
---

## Intro
I spend most of my professional time doing code: writing, deleting, reviewing and _thinking_ code. I do this at seats.io, a small company that I co-founded and that does floor plans for online ticketing websites.    

Our product is "just" a set of tools to create and edit floor plans, to embed them in a host page, and an API to manage pretty much everything related to floor plans. 

Our customers embed a piece of javascript, do:  

```new seatsio.SeatingChart(config).render()``` 

and we show an interactive floor plan of the venue in their ticket selling page. 

Two observations. First, if seats.io does not work for whatever reason, the ticket buyer cannot buy tickets. We directly impact our customer's bottom line, so it's of the utmost importance that seating charts "just" work.  
Second, they have to work on all (modern) browsers, on all platforms.   

Obviously, there's much more to this, but that's more than enough for you to be able to read the rest of this post. One day, I'll go into detail, but It Is Not This Day!

// TODO aragorn picture

## Zooming
We've been around since early 2014, and since those early days, zooming seating charts has been a struggle. We've had a `MouseWheelNormalizer` class in the code ever since and rewrote it at least 2 times.

You can imagine how delighted I was recently, when I found out that pinch-to-zoom on Firefox/Mac seemed 




