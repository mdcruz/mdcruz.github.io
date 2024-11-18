---
author: "Marie Cruz"
title: "What To Consider When Doing Visual Testing"
date: "2021-09-16"
tags:
- testing
- thoughts
- visual-testing
---

If you are working on a front end application whether it’s website or mobile, visual testing is a great one to add in your testing strategy. Apart from choosing the visual tool, there are so many things to consider when doing visual testing and in this post, I will share some tips that I have found useful when implementing visual testing on various projects.

## Choosing the Right Tooling

The first thing to consider is choosing the right tool for the job. There are visual testing plugins that integrate directly to your testing library or framework of choice. Some allow you to leverage your functional tests while some allow you to write new tests entirely. Depending on which library or framework you are using, choosing the right tooling is an essential aspect when it comes to adding visual testing to your test strategy. Nowadays, there are many visual testing tools to choose from paid tools such as [Applitools](https://applitools.com/), [Percy](https://percy.io/), [Happo](https://happo.io/) or [Chromatic](https://www.chromatic.com/) to open source libraries such as [AyeSpy](https://github.com/newsuk/AyeSpy), [BackstopJS](https://github.com/garris/BackstopJS), [Wraith](https://github.com/bbc/wraith), [cypress-image-snapshots](https://github.com/jaredpalmer/cypress-image-snapshot) and more. With each of the tooling that’s available, you need to do an analysis as to which will meet your testing requirements the most. 

## Level of Visual Testing

I see visual testing as a combination of both component and full page testing. Component visual tests focus on individual components and its different variations while full page testing focuses on the entire page that our user sees. You need to consider in your testing strategy if you’re going to support both levels or not. Depending on the type of system that you are working with, you might consider component visual testing very important when it comes to Design Systems. On the other hand, full page visual testing is very important on high traffic pages. 

## Browser Support

Cross browser visual validation is important especially if you have a wider user base. Nowadays, the code that gets deployed to different browsers does not cause a lot of functional differences but visually, there could still be elements that render differently. As part of your testing strategy, you need to decide which browsers you are going to support when you do visual testing. Depending on your tooling of choice, some visual tools support multiple browsers while some tools only let you run visual validation on a single browser. Will you automate cross browser visual validation or will you adopt a hybrid approach where you run automated visual tests on a single browser and then check other browsers manually?

## Test Maintenance

Test maintenance is an activity on its own that often gets overlooked. Once you have written the visual tests, you also need to make sure that you maintain them as the application’s code base gets refactored or changed. From an ownership point of view, who’s going to maintain these tests? Is it the entire development team? If so, you need to make sure that the visual testing tool integrates smoothly in their development workflow and they can easily write visual tests.

Some visual testing tools also don’t require a lot of test maintenance when adding new visual tests. If you are using visual testing tools that integrate with Storybook for example, any new components that your team adds will be automatically tested.

Another thing to consider is who’s going to approve the visual differences? Do you also want your design team to approve and reject visual changes? If you want to open this to more people, this is where an easy to use visual testing dashboard where team members can easily approve or reject visual changes will come in handy. 

## Pipeline Integration

What is your plan of action when integrating visual tests to your Continuous Integration pipelines? Will you run visual tests on every pull or merge request in order to get early feedback or will you only run it after it’s been deployed to a QA or Staging environment? When running visual tests, will you run it on every commit or as a job that gets triggered manually by the person raising a request? 

## Developer Experience

I feel that this is one of the most important things to consider. When introducing new types of testing, we need to make sure that developer experience has not degraded. In order to get their buy in and for these tests to be valuable, their workflow should not be disrupted as much. Getting your developers’ opinions is really crucial and it’s important to consider all possible scenarios when integrating visual tests. As an example, we have integrated visual testing into one of the projects in my current company. We ran the tests on our pipeline for about a month and gathered useful metrics and feedback from different engineers. Rather than sticking to a solution that just works, we took everyone’s feedback and are now planning to do another iteration on how we can improve the visual testing experience. It’s all about a trial and error approach.

## Baseline Images

Visual testing works by comparing test images to a set of baseline images which serve as your source of truth. How will you store these baseline images? The paid plugins allow you to store it on their servers while with the free plugins, you have to think on how you will share the baseline images to the team. You can commit it to your github repositories but imagine if you have hundreds of baseline images, it's not going to be an ideal solution. You have to either store it on a storage service such as an S3 bucket or something similar so additional work will be needed. Speak to your development team on what would be the ideal solution here.

When it comes to approving baseline images, how would you handle scenarios where multiple merge or pull requests update the baseline images at the same time? Some visual testing tools allow for a mature baseline branching strategy while other tools have limited or no support. 

## Handling Dynamic Data

To avoid a lot of false positives and failures on your visual tests, what is your action plan to handle dynamic data? Since most of the visual testing plugins use pixel-by-pixel comparison algorithms, you need to make sure that the page you are testing is free of dynamic data as much as possible. Some techniques here include using data fixtures to mock the data, pausing animations and using static date and time. If you want to compare dynamic data, tools such as Applitools uses AI algorithms to compare images just like a real human would do. Alternatively, if you and your team have decided that dynamic data is to be excluded, these visual testing tools have a way of excluding particular elements when the snapshot process is performed.

## Budget and Time Constraints

What are your budget and time constraints? You need to weigh the pros and cons of each tool and decide if the tool will deliver more value than cost. This is definitely needed when evaluating paid visual testing tools. If possible, create a budget forecast for the upcoming years. Apart from the monetary cost, evaluate as well the time and effort it will take to integrate these tools. From a cost perspective, you need to analyse carefully if the features that these tools provide are cost effective and cheaper in the long run.

## Reporting

You need to also consider how the visual testing tool integrates with other tools for reporting and visibility. When there are visual changes to be reviewed, how would you communicate this to your team? If your team is using a business communication platform such as Slack, it’s a good idea to set up automated notifications on a dedicated slack channel so it’s visible to everyone involved.

## Community and Maintenance Support

What is the community and maintenance support for the visual tool? Are there lots of tutorials around the tool? When was it last updated? These are important questions to consider as you and your team look for further advice on what the community has done. The great thing about paid plugins is that there will be dedicated technical support to help you with any specific queries you might have. When choosing a free plugin, check what the weekly downloads are for the tool and what sort of open issues they have. As some visual tools integrate with your testing framework of your choice, it’s a good idea to know if these plugins are also updated whenever a major version of your testing framework is introduced.

## Wrapping Up

Remember, there are so many things to consider when choosing which visual tool to choose. Each tool has their own strengths and limitations. So, choose the tool that will best work with your requirements. Don't just pick a tool without doing a lot of research and proof of concept. Foster a trial and error approach and experiment with different tools to see which meets your testing needs the most.



