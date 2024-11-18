---
author: "Marie Cruz"
title: "Pillars of a Good Test Automation Framework"
date: "2021-12-13"
tags:
- testing
- thoughts
---

Last month, I participated on a panel event from [Perfecto User Group](https://www.perfecto.io/resources/events/perfecto-user-group-nov-overview) where we talked about emerging test automation frameworks. 

There were a lot of excellent points raised by everyone from the panel and I wished we had more time to discuss all the points but of course, we had limited time. One of the main discussions that we talked about is the pillars of a good test automation framework. In this blog post, I want to share some of the discussion that we've had as well as my personal opinion as to what I think makes up a good framework.

## Reusability

One of the qualities that makes up a good test automation framework is reusability. When you are able to reuse certain code or utilities rather than writing the same functionality again, it saves everyone time. Examples of reusability include sharing or importing common classes or functions to different parts of your code, reusing a set of test data and moving common steps to a test hook such as a before hook or a before each hook.

If you write a code with reusability in mind, the code is easier to read and used by other engineers. You'll also get more buy-ins from developers especially if you are encouraging them to help write the automated tests.

## Maintainability

Have you ever worked on a test automation framework where the effort to maintain the tests is much higher than actually writing the test? Maintainability simply refers to how a framework can be easily modified or extended if there are future changes to it. Following a set of good coding standards and patterns can help improve the readability of your code, which then leads to maintainability. A framework that has low coupling meaning that a change on one part of your code won't impact or break different parts of your code can also help improve maintainability.

## Scalability

A good test automation framework should be able to scale up well when the number of tests increases. When starting out, having a few tests should not have any speed issue but what if you've scaled it to hundreds or even thousands of tests? This is where having a good test automation design and organisation really helps. The tests should also not be brittle or flaky so practices such as using custom data attributes or waiting for network calls rather than hard coding additional waits can help. The framework should also be able to handle running tests in parallel and shouldn't fail when one test runs over another.

## Framework Extensions

Another pillar of a good test automation framework is its ability to extend easily through various use of plugins and/or libraries. For example, I've personally advocated the use of Cypress to leverage it beyond functional testing due to its vast collection of plugins. Plugins for accessibility, performance, visual and code coverage, to name a few are some of the example plugins that you can use to extend your Cypress framework.

Nowadays, reporting is also another key feature of what makes a good test automation framework. You can now integrate various reporting plugins and third party integrations to communication channels such as Slack to make the test reporting more visible.

## Ease of Use/Adoption

To get buy-ins from engineers, developer experience is key when it comes to introducing a new automation framework. The framework should be easy to use and it should have an easy onboarding experience. Depending on which automation framework you choose, it should have the ability to debug tests easily and engineers should spend most of their time just writing the tests rather than trying to figure out how it works.

## Technical/Community Support

What is the technical support for the test automation framework that you are using? If using paid tools, the technical support is there. However, when choosing an open source tool, is it backed up with excellent community support? If you encounter specific issues, how easy is it to find resources to help you debug the issue that you are having? This also applies to choosing plugins for your framework. When selecting a plugin, what is the weekly downloads or general usage? Are there a lot of open issues that need to be fixed in the plugin and also when was the plugin last updated?

## Wrapping Up

From the above, there are a lot of qualities and features that make up a test automation framework. Having a solid framework makes it manageable to write automated tests and by having the qualities above built in, you can scale your tests when your team expands. The above isn't the complete list but should give you enough guidance. 