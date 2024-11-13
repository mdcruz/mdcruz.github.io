---
author: "Marie Cruz"
title: "Hunting Production Bugs: Month of April"
date: "2020-04-20"
tags:
- testing
- bugs
---

Because of the current COVID-19 situation, my screen time has massively increased the past few weeks. I’ve been visiting different sites than usual and accidentally finding production bugs that slipped. 

I’m thinking of starting a series of posts (probably every month) where I share production bugs that I’ve encountered. These bugs will range from server error bugs, visual bugs, functionality bugs, etc. I am not shaming these companies but I just want to raise awareness that there is no such thing as 100% bug free application - and it’s ok!  

## Production Bug #1: Blooming Artificial

Couple of weeks back, I was trying to order a plant from Blooming Artificial. I found a promo code that I wanted to use but when I clicked checkout, the following error occurred. When I refreshed the page, the error disappeared but my shopping cart was empty.

<figure>
  <img src="../../images/blooming-artificial.png" alt="Blooming artificial server error">
</figure>

## Production Bug #2: Daily Record

Here’s an example of a visual bug of an advert overlapping with the news content. It didn’t mind me as much because I could still read the content but visually, it was a bit annoying to look at 😅

<figure>
  <img src="../../images/daily-record.png" alt="Daily record mobile website">
</figure>

## Production Bug #3: Coop

While buying groceries online, I noticed a couple of bugs from the coop website. I set my filters to browse long life milk and the filter count said 9 of 8 products. Not sure where the 9th item came from but I counted the products manually and there were only 8.

<figure>
  <img src="../../images/coop.png" alt="Coop mobile website">
</figure>

The following day I decided to visit the site again because I forgot to add a couple of items in my grocery list. It seems that even though my basket is currently empty, the price for the previous items that I bought is still displayed.

<figure>
  <img src="../../images/coop2.png" alt="Coop mobile website">
</figure>

## Production Bug # 4: Daily Mail

Browsing the online papers again and this time, I was looking at Daily Mail. I wanted to see their most watched News videos so I swiped right only to find empty blocks.

<figure>
  <img src="../../images/daily-mail.png" alt="Daily mail mobile website">
</figure>

## Production Bug #5: Cypress Docs Site

I was looking at the Cypress documentation site to look at examples on how to use one of their commands. There was nothing functionally wrong but in terms of readability, the example code was difficult to read because it wasn’t formatted properly. The keyword javascript is also added at the beginning. Only seems to be an issue on mobile devices though as desktop version is returning the format as expected.

<figure>
  <img src="../../images/cypress-docs.png" alt="Cypress docs mobile website">
</figure>

These all shows that we're all human, it's ok to slipped bugs in production (but not all the time!!). I know I have a fair share of bugs I've deployed in the past. 😀