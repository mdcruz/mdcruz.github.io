---
author: "Marie Cruz"
title: "Shifting Accessibility Testing to the Left"
date: "2021-08-31"
tags:
- testing
- thoughts
- accessibility
- applitools-blog
---

This post was originally published on the [Applitools](https://applitools.com/) blog which you can view [here](https://applitools.com/blog/shifting-accessibility-testing-to-the-left/).

15% of the world’s population live with a form of disability. That’s equivalent to about one billion people. As we now live in a digital era where everything can be accessed online, we need to make sure that the features we provide are accessible to everyone, including those with disabilities. Accessibility testing is definitely not something new – it’s something that we’ve been trying to do for many years now. Yet until now, the number of inaccessible websites that we encounter are still very high.

## The Accessibility Problem

Speaking with different individuals, it seems that there’s still a lack of buy-in from various teams to prioritise designing, developing and testing for accessibility. We’ve all heard about the infamous [Domino’s accessibility case](https://www.bbc.co.uk/news/technology-46894463). There are regulations out there that state that organisations must have a responsibility to make sure that their products are accessible by people with disabilities. However, based on [WebAIM’s accessibility research](https://webaim.org/projects/million/) called the WebAIM Million, where they analysed the homepages of the top one million websites every year, they stated that “Users with disabilities would expect to encounter detectable errors on 1 in every 17 home page elements with which they engage.” From their research, they approximately found 51 million distinct accessibility errors. While the results for 2021 are slightly better than previous years, a significant chunk of work still needs to be done to make sure that the web is accessible for everyone.

There are a lot of available tools out there that can help us with accessibility testing. Giridhar Rajkumar has previously blogged what are some of the accessibility testing tools we can use to help us with testing ([Accessibility Testing: Everything You Need To Know](https://applitools.com/blog/accessibility-testing-strategy/)). However, the statistics above show that there’s still a lot of work to be done. In this blog post, I’ll explain why designing for inclusivity wins, why we need to talk about accessibility a lot, what we can do to create a culture of inclusivity and how to shift accessibility testing to the left.

## Designing for Inclusivity

When we design for inclusivity, everyone wins. Let’s look at an image below to explain this concept.

<figure>
  <img src="../../images/equality.jpg" alt="Three images showing people watching a baseball game. The first image shows a non see through fence, second image with the same fence but people have additional boxes to stand on and the last image shows a see through fence.">
  <figcaption>Everyone can enjoy the baseball game once the barriers have been removed and the fence is see through</figcaption>
</figure>

Basically what we have here is we have a baseball game and a fence separating the people watching it and to also stop people running to the baseball field. Not everyone can see through the fence unless you are tall enough. This represents inequality. To remedy this, on the first image, we can give everyone a box to stand on even if they don’t need a box to emphasise equality. Some people will be able to watch along just fine but others won’t.

The second image shows that some people who are not tall enough can ask for additional boxes so they can watch the game. However, this is bad equity because someone needs to be there to hand out boxes physically which can be seen as an extra business overhead.

In order to not increase business overhead, we can just make the fence see through like in the last image. It still keeps people from stepping into the baseball field and at the same time, everyone can watch the game without anyone handing them boxes. Everyone wins if you think about being inclusive. Rather than seeing this as a cost overhead, it’s now become a revenue generator because everyone, whether you’re tall or small, in a wheelchair or not, can enjoy the game.  

## Talking About Accessibility

So why is it that so many teams still think accessibility is a nice to have feature rather than having it embedded right from the beginning? I think the answer is we don’t talk about accessibility a lot. It’s not because our teams can’t code accessible websites, but because there is a missed education around the importance of accessibility which then leads to no consideration at all for accessibility. By educating our teams the benefits of having accessible products, we can get them to understand why we need to include it in the first place. 

We need to educate our teams that accessible products are easier to use and it increases user satisfaction. The more users a business can reach, the better it is for the business since it will have financial benefits. Accessibility is also linked to higher SEO ranking and better quality code. It avoids lawsuits and legal complications and encourages independence amongst all our users.

## Creating A Culture Of Inclusivity

There’s all kinds of areas where we can bring value and make things more accessible but it starts from creating a culture first of inclusivity. This book that I read by Regine Gilbert called [Inclusive Design for a Digital World](https://www.amazon.co.uk/Inclusive-Design-Digital-World-Accessibility/dp/148425015X), which I highly recommend to everyone, explains how it takes a whole village to make accessible products. It’s not only up to us, it’s up to everyone. 

Accessibility tools are here to help us and it can provide value but if there is no change in mindset and culture, then shifting accessibility testing to the left will be difficult since you’ll get resistance from the business to prioritise it. 

## Shifting Accessibility Testing to the Left

In order to shift accessibility testing to the left, we need to make sure that accessibility is part of every stage of the development lifecycle and even as far left as possible from the planning stages. Rather than reacting to it right at the end, proactively discuss it as early as possible and get buy-in right from the start. Get your project managers involved and your leadership team onboarded so they know that it’s a priority and educate them that if it’s left out right at the end, it can be time consuming to fix the accessibility issues and exponentially expensive. 

Shifting accessibility is not just about automation. It’s also about having these conversations earlier on the cycle and trying to influence everyone in your team and the business so that everyone is on the same page.

Apart from early conversations and using automation to catch basic accessibility issues, the following are some of the example activities that you can do to shift left:

- Sit with your UX and Design team and test the design mock-ups. Verify if the general layout is clear and the colours use high contrast.
- Participate in planning sessions and contribute what scenarios need to be taken into consideration. This is where adopting different personas can help.
- Plan ahead on how it should be used by keyboards and screen readers and share that knowledge to your team.
- Participate in code reviews if you can. Semantic tags have built-in accessibility features so make sure that these are used appropriately.
- Try pair testing to catch more accessibility issues earlier on. 
- Integrate automated tests as part of your continuous integration pipelines so you can provide faster feedback to your team.

When accessibility is embedded right from the start, user experience will improve and be simplified and barriers for people with disabilities will be removed.

## More Resources

Accessibility can be overwhelming, there are tons of resources out there with lots of information. The [Web Content Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) contain a lot of valuable information but it can be daunting to absorb all that information especially if this is your first time. 

So, what can you do? Start talking to your teams about accessibility! Find your allies and get that buy-in from the business by explaining the benefits. Remember that it needs a shift in culture and mindset. Once you have that support, if your website is live already, it’s a good idea to first see how accessible it is. Have an accessibility audit performed so you can see where the issues are. Start with a single page and acknowledge that it will take time. You won’t solve all the issues at once but that is ok. Every small accessibility fix that you and your team introduce will make it better for someone out there.

If you want to know more about testing for accessibility, check out this free course from [Test Automation University – Test Automation for Accessibility](https://testautomationu.applitools.com/accessibility-testing-tutorial/).



