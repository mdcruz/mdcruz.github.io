---
author: "Marie Cruz"
title: "Testing Iframes with Cypress"
date: "2020-07-11"
tags:
- testing
- tools
- test-automation
- cypress
---

This is an accompaniment post to a Cypress lunch and learn series that I've done previously. An example github repo which contains the different code examples can be found [here](https://github.com/mdcruz/cypress-iframe-example).

## Cypress' limitations with iframes

One of the limitations with using Cypress is about handling iframes. First of all, Cypress can't access iframes directly. Unlike other testing tools where you have built in commands to switch to an iframe directly, there is currently no built-in method to do it with Cypress. The other challenge is that when you debug your tests using the Cypress Test Runner, the iframe snapshots is not displayed which makes it hard to debug our tests.

## Why iframes?

Why do we need to use iframes though? Iframes are used to load or embed external content such as videos or images easily to your websites. When you embed an iframe, this is loaded separately from your basic HTML document markup. 

The advantage of using iframes is that users will still be able to see external contents on your website even if the rest of your web content has not loaded yet. It's also a great way to keep your users in your website directly because they don't have to visit other websites to view the external content. On the other hand, disadvantages of using iframes include issues around security and it can sometimes slow down your page if the iframe content is slow to load.

## Can we still use Cypress to automate iframes?

We can still automate them using Cypress. However, we will need to use some custom code to make it work. In this post, we'll be looking at automating this [iframe demo app](http://the-internet.herokuapp.com/iframe).

<figure>
  <img src="../../images/heroku-app.png" alt="The internet demo application">
</figure>

## Writing our first test

Let's create a spec file called `iframe.spec.js` and add our code first to visit our test application.

```
it('should let you switch to the iframe', () => {
  cy.visit('http://the-internet.herokuapp.com/iframe');
});
```

Notice though that when we open the test runner, you'll see that when you hover or click on the test command, the iframe itself disappears, which is one of the limitations unfortunately.

<figure>
  <img src="../../images/iframe-cypress.gif" alt="Visiting an application with iframes in Cypress">
</figure>

Anyway, let's add the code snippet that we can use to switch and interact with the iframe and break it down one by one.

```
it('should let you switch to the iframe', () => {
  cy.visit('http://the-internet.herokuapp.com/iframe');
  const iframe = cy
    .get('#mce_0_ifr')
    .its('0.contentDocument.body')
    .should('be.visible')
    .then(cy.wrap);

  iframe.clear().type('hello');
});
```

We are first getting the iframe selector which is `#mce_0_ifr`. Then, to access it, we need to use `0.contentDocument.body`. In JavaScript, $0 returns the most recently selected element which is the iframe in this case. The contentDocument returns the document object that the iframe generates and from it we just access its body and assert that it's visible. Afterwards, we make a call to `cy.wrap` which is a way in Cypress to chain commands off it. 

If we run the test again, we should see that we can now interact with the iframe. 

<figure>
  <img src="../../images/cypress-iframe-pass.png" alt="Passing test with iframes in Cypress">
</figure>

## Custom commands

It's a good idea to use a custom command for switching to an iframe because this could be used multiple times. So let's go ahead and create a custom command for it.

```
Cypress.Commands.add('switchToIframe', (iframe) => {
  return cy
    .get(iframe)
    .its('0.contentDocument.body')
    .should('be.visible')
    .then(cy.wrap);
});
```

Then on our test code, we can call the custom command instead.

```
it('should let you switch to the iframe', () => {
  cy.visit('http://the-internet.herokuapp.com/iframe');
  cy.switchToIframe().clear().type('hello');
});
```

## Cross domain iframes

The above solution would only work if our iframe is within the same domain. I've added another spec file in the example github repo which demonstrates this. In this example, we are loading the [todo app](http://todomvc.com/examples/react/) as an iframe which is a different domain from our localhost.

<figure>
  <img src="../../images/cross-iframe.png" alt="Failing test with cross domain iframes in Cypress">
</figure>

If we are testing iframes from different domains, we need to tell the browser that it's safe to access them and this can be done by setting `chromeWebSecurity` to false in your `cypress.json` file. Once this is set to false, then the test will pass. 🎉

## Using cypress-iframe plugin

If you don't want to create a custom command, you can also try using the [cypress-iframe](https://www.npmjs.com/package/cypress-iframe) plugin. To set this up, simply install the package and add the following import or require statement in your `cypress/support/commands.js` file.

```
import 'cypress-iframe';
// or
require('cypress-iframe');
```

To load the iframe, you need to use the `cy.frameLoaded()` method and pass the iframe selector. This method will first verify if the iframe is loaded in the page. Then you just need to call `cy.iframe()` and chain the commands that you want to execute which you can see in the code snippet below.

```
it('should let you switch to the iframe using the cypress iframe plugin', () => {
  cy.frameLoaded('#mce_0_ifr');
  cy.iframe()
    .clear()
    .type('hello')
    .should('have.text', 'hello');
});
```

## Wrapping up
Even though Cypress has limitations with iframes, there are ways to work around with it. Apart from what I've shown above, there are also other workarounds from other people that you can use to test iframes which is documented on this [issue](https://github.com/cypress-io/cypress/issues/136). I do hope that the Cypress team addresses the issue though of the iframe disappearing when you debug it in the test runner!