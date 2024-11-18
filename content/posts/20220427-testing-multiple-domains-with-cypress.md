---
author: "Marie Cruz"
title: "Testing Multiple Domains with Cypress"
date: "2022-04-27"
tags:
- testing
- tools
- test-automation
- cypress
---

As you probably might have heard already, Cypress has released version 9.6.0 which finally allows testing for multiple domains! This is probably one of the most requested features from the community (support visiting multiple super domains in one test) and looking at this issue raised in their Github repo in 2017, it was pretty exciting to see that this is now supported in their latest release. Really well done to the Cypress team for getting this out! 🥳

In this quick blog post, I'll look at how to use the new command `cy.origin()` and show you how you can test multiple domains with this experimental command.

## Use cases for multiple domains

Cypress has documented some [common workarounds](https://docs.cypress.io/guides/guides/web-security#Common-Workarounds) on how users can test scenarios encompassing multiple domains but if we want to automate the scenarios from how our users will use it, it's still a good idea to automate it from the UI. Be mindful though that driving everything from the UI has its own challenges so it's still a great idea to utilise the workarounds and leveraging API calls if possible.

Common use cases for testing multiple domains include:

- Visiting more than one super domain as part of your test
- Interacting with components that are from a cross-origin iframe
- Payment workflow journey using third party applications
- Login authentication using third parties

## Getting Started
To get started, you need to update the Cypress version that you are using to version 9.6.0. Once you have done that, you need to enable `experimentalSessionAndOrigin` in your cypress.json file.

## Introducing cy.origin()

Let's look into a scenario where you need to visit another super domain as part of your test. 

```
it('should redirect me to the new domain', () => {
   cy.visit('zoopla.co.uk');
   cy.visit('https://www.mariedrake.com/blog');
   cy.get('#SITE_HEADER').should('be.visible');
});
```

Before `cy.origin()` was introduced, this was a hard limitation and you are faced with the dreaded cross-origin error if you are trying to visit two super domains in a single test.

With the experimental `cy.origin()` command, you can now get rid of this error. Let's have a look at how we can achieve this with the code snippet below.

```
it('should redirect me to the new domain', () => {
   cy.visit('zoopla.co.uk');
   cy.origin('mariedrake.com', () => {
     cy.visit('/blog');
     cy.get('#SITE_HEADER').should('be.visible');
   });
 });
```

The `cy.origin()` accepts two arguments by default. The first argument is the URL specifying the secondary origin in which the callback is to be executed. Note that this is just setting the origin of the new domain and not visiting the new domain as of yet. The second argument is the callback to be executed which contains additional actions that I want to do in the new origin. 

There is also an optional and third argument that you can pass which can be used for passing in additional configurations to your origin callback. For example, in the code snippet below, I am passing the SITE_HEADER variable instead and also explicitly passing this to my callback function. The reasoning for this has been documented here - [Using dynamic data in a secondary origin](https://docs.cypress.io/api/commands/origin#Using-dynamic-data-in-a-secondary-origin).

```
it('Then it should redirect me to the new domain', () => {
   const SITE_HEADER = '#SITE_HEADER';

   cy.visit('zoopla.co.uk');
   cy.origin('mariedrake.com', { args: SITE_HEADER }, (SITE_HEADER) => {
     cy.visit('/blog');
     cy.get(SITE_HEADER).should('be.visible');
   });
});
```

When I run my test, I should see that it's now passing and I'm now able to successfully visit multiple super domains.

Note that adding additional commands outside the cy.origin() code block will execute actions against the original origin (the original domain).

## Feedback

Since this is an experimental feature, the Cypress team needs your feedback if there are any issues that you are facing while trying out cy.origin(). There's already some issues logged with Microsoft and Google authentication so if you have any feature requests, log it as part of this [Github issue](https://github.com/cypress-io/cypress/discussions/21186).

## Resources

For further reading and resources, check out the following links:

- [Cypress 9.6.0: Easily test multi-domain workflows with cy.origin](https://cypress.io/blog/2022/04/25/cypress-9-6-0-easily-test-multi-domain-workflows-with-cy-origin/)
- [Multi-Domain (Origin) Testing with Cypress](https://dev.to/cypress/multi-domain-origin-testing-in-cypress-1aog)
- [Cypress origin API documentation](https://docs.cypress.io/api/commands/origin)




