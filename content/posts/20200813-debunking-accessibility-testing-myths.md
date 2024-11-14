---
author: "Marie Cruz"
title: "Debunking Accessibility Testing Myths"
date: "2020-08-13"
tags:
- testing
- thoughts
- accessibility
---

Recently, I sent out a testing and quality survey at work to understand any pain points that other teams have on their testing process and one of the feedback I got was around accessibility testing. While we are already doing accessibility testing within the team that I am on, I've realised that other teams are still unsure on how to approach accessibility testing.

The most common excuse that I often hear is they don't have the skills or the time to do accessibility testing. While I admit that accessibility testing can be overwhelming especially if you are just starting out, there are tons of great resources out there to help you get started which I will link down below. On this blog post, we'll look at what are some of the accessibility testing myths that people might have and how we can debunk it.

## Myth #1: We don't have time to do Accessibility testing 

There is this common misconception that teams don't have the time to do accessibility testing because of feature deadlines and other activities. However, if accessibility testing is embedded at the beginning, then this shouldn't be the case. If you're only thinking of doing accessibility at the end after a feature has been deployed, then yes it's time consuming and will end up being ignored... So, consider discussing with your team the benefits of having an "Accessibility First" approach. We need to be emphatic to our users with disabilities and put ourselves in their shoes so think of this the next time you find yourself saying that you don't have time to do accessibility testing. If you're in a new project, then include accessibility earlier on. Nowadays, there are a lot of tools out there that can help you with testing and some of these tools allow you to automate some of the basic accessibility checks which can save you time in the long run.

<figure>
  <img src="../../images/accessibility.png" alt="Accessibility testing embedded on the entire software lifecycle">
  <figcaption>How to fit accessibility into Software Lifecycle</figcaption>
</figure>

The image above from deque shows how you can fit accessibility on the different processes your team might have and not seen as an afterthought. When embedded earlier on, testing for accessibility should not be time consuming.

## Myth #2: Accessibility is the responsibility of testers

I guess this isn't entirely just about accessibility but quality in general. It's not just your responsibility, it's everyone else's including the developers, designers and everyone else involved. However, does this mean that we don't need accessibility experts or specialists? Wrong. On some cases, we still need them to guide us and share their wealth of accessibility knowledge. However, everyone should have an understanding that it's their responsibility to deliver a high quality and accessible product. On every decision made, accessibility should be kept in mind. For example, let's take a news website. If you've got people from the technology team testing for accessibility however the editorial team doesn't add alternative texts to images, then you're back in square one. Everyone needs to be educated and be aware of accessibility.

## Myth #3: Automated accessibility tests replaces actual accessibility testing

This is completely untrue. Just because your automated accessibility tests is passing, this doesn't mean that your website is accessible at all. While there are so many great accessibility testing tools out there, only between 20%-40% of accessibility issues can be caught by these tools. I recommend this [UK Government accessibility article](https://accessibility.blog.gov.uk/2017/02/24/what-we-found-when-we-tested-tools-on-the-worlds-least-accessible-webpage/) where they shared their insights on testing the world's least accessible web page with different automated tools and this shows that human intervention is still very much required when doing accessibility testing. This shouldn't dissuade you though to use automated tools to help you with accessibility testing as they are still very valuable, cheap, fast to run and it's great for giving early feedback. Just don't rely on them 100% of the time.

## Myth #4: Accessibility testing is hard

Some might disagree with me... but accessibility testing is not hard once you make use of the available resources out there. I started my career as a software tester not focusing much on accessibility testing and I had this perception that accessibility testing is hard. However, once I educated myself and learn about the different tools and resources that are available, it's not that hard after all. If you don't have experience at all with accessibility testing, start with the basics. Play around with your keyboard and don't use a mouse for navigating through a website and see if you can access it the same way as if you are using a mouse. If you're a mac user, the [VoiceOver](https://www.apple.com/uk/accessibility/mac/vision/) screen reader is built in already so make use of it and see if the tool is reading out the descriptions well and if it makes sense. On other operating systems, [NVDA](https://www.nvaccess.org/) screen reader is pretty much the popular screen reader tool out there. Have a look at [W3C's easy accessibility checks](https://www.w3.org/WAI/test-evaluate/preliminary/) and use this as a starting point. Tools such as [Axe](https://www.deque.com/axe/), [Wave](https://wave.webaim.org/), [Google Lighthouse](https://developers.google.com/web/tools/lighthouse) all have browser extensions that you can easily use to detect accessibility issues you might have so utilise these. 

## Myth #5: Your product is accessible because it's WCAG (Web Content Accessibility Guidelines) compliant

Just because you achieved level AA and is WCAG compliant, it doesn't mean that your product is 100% accessible. Different people with different disabilities will have different ways of using your product and sometimes the rules listed on WCAG doesn't always represent the way they use your application. It's a great guideline to use but you should use it in conjunction with actual user research and inviting real users to do accessibility testing. Also there is no such as thing as 100% accessibility rating!  

## Resources
If you are looking to learn more about accessibility testing, you can also check out the following resources below:

- [W3C Web Accessibility Initiative](https://www.w3.org/WAI/)
- [Web Accessibility Guidelines by James Sheasby Tomas](https://inviqa.com/blog/web-accessibility-guidelines-compliance-inclusive-design)
- [Accessibility Testing Tools for Automation by Joe Colantonio](https://testguild.com/accessibility-testing-tools-automation/)
- [Getting Started with Accessibility by Emily Mears](https://medium.com/@emilymears/getting-started-with-web-accessibility-2c7632c3a8bd)
- [Getting started with Cypress-Axe](https://www.youtube.com/watch?v=oSzW6bQTDDk&t=1686s)
- [Wuhcag](https://www.wuhcag.com/)
- [A11y with Lindsey](https://www.a11ywithlindsey.com/)