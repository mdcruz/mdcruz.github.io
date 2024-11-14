---
author: "Marie Cruz"
title: "API Testing with Cypress"
date: "2020-06-29"
tags:
- testing
- tools
- test-automation
- cypress
---

This is an accompaniment post to a Cypress lunch and learn series that I've done previously. An example github repo which contains the different code examples can be found [here](https://github.com/mdcruz/cypress-api-testing-framework).

As test automation engineers, we often write two set of tests. UI and API tests. API tests are normally run on the command line and browser tests are run on the UI. From my personal experience, every time we have a project where we have to do both UI and API testing, we always use separate tools. This can be a good thing since we always have to make sure we expand our testing toolbox so we know which one fits our requirements the most however, there are so many tools out in the market to help us with API testing. Probably the most popular in my opinion is Postman since we can play with the requests graphically. If you need to run your Postman tests on a CI pipeline for example, you will need to use Newman, which is their test runner equivalent. Other JavaScript testing framework popular include SuperTest and Chai-HTTP. On the Java world, we have Rest Assured and Karate just to name a few.

But what if I tell you that we can use the same tool for UI testing to test our APIs? That we can add a visual test runner for our API tests and also run it headlessly on the command line?

What's great with Cypress is that it has built in support for making requests to our servers. Because we have access to the test runner, this makes it easy to debug our API tests. We can always revisit the state of our APIs and we also have access to DevTools which can contain more information that could be helpful for our tests. 

## cy.request()

```
cy.request(url)
cy.request(url, body)
cy.request(method, url)
cy.request(method, url, body)
cy.request(options)
```

By default, cy.request() equates to doing a GET request. But, this is very flexible and supports all the HTTP actions such as POST, DELETE, PUT. 

## Testing JSONPlaceholder

Let's write a couple of tests against this example API http://jsonplaceholder.typicode.com/. We can test the /todos endpoint and the code snippet below shows a very simple example on how to use the cy.request().

```
/// <reference types="cypress" />

describe('Todo API', () => {
  it('returns a JSON data', () => {
    cy.request('/todos')
      .its('headers')
      .its('content-type')
      .should('include', 'application/json');
  });
});
```

With this example, we are making a GET request to the /todos endpoint and then checking its headers and within headers, we are checking it's content-type and asserting that it should include 'application/json'.

Let's also write some tests on the /users endpoint and just quickly verify its status code and the number of users we have. We can leverage aliases in Cypress as a way to refer back to the request.

```
/// <reference types="cypress" />

describe('Users API', () => {
  beforeEach(() => cy.request('/users').as('users'));

  it('return the correct status code', () => {
    cy.get('@users').its('status').should('be.equal', 200);
  });

  it('should return the correct number of users', () => {
    cy.get('@users').its('body').should('have.length', 10);
  });
});
```

Now when we open our Cypress Test Runner, every time you click on any of the commands issued on the left hand side, additional logs will be displayed on the console, which can be super helpful when it comes to debugging a test.

<figure>
  <img src="../../images/cypress-test.gif" alt="Cypress test runner in action">
</figure>

What about other methods like doing a PATCH request? This can also be easily tested with Cypress. Let's look at the two tests below.

```
/// <reference types="cypress" />

describe('Todo API', () => {
  it('marks todo item as completed', () => {
    cy.request('PATCH', '/todos/1', { completed: true })
      .its('body')
      .its('completed')
      .should('be.equal', true);
  });

  it('marks todo item as completed - different way', () => {
    cy.request({
      method: 'PATCH',
      url: '/todos/1',
      body: {
        completed: true,
      },
    })
      .its('body')
      .its('completed')
      .should('be.equal', true);
  });
});
```

The two tests above does the same thing - it marks a todo item as completed but I want to show different ways of doing it with Cypress.

The first test accepts three parameters. The first parameter is the word 'PATCH' to indicate that this is for a PATCH request. The second parameter is the endpoint that we need to update and the third parameter is the request body that we want to send. In this particular example, we want to set completed to true. 

The second test only accepts one parameter and it's an object. In this object, we are passing all the relevant values such as its method, url and request body. Since the data does not persist in this example API, for our assertion, we're just checking if the response is set to true for the completed property.

You can find more information about `cy.request()` [here](https://docs.cypress.io/api/commands/request.html#Syntax). 🙂 

## Opening DevTools automatically 

Since there is no application preview that we can see visually with API testing, I find that it's better to automatically open DevTools console when you load the Cypress test runner. Nothing wrong with opening it manually but if you want to save 1-2 seconds then this can be easily achieved by extending `plugins/index.js` and adding the code snippet below.

```
/// <reference types="cypress" />

/**
 * @type {Cypress.PluginConfig}
 */
module.exports = (on) => {
  on('before:browser:launch', (browser = {}, launchOptions) => {
    if (browser.name === 'chrome') {
      launchOptions.args.push('--auto-open-devtools-for-tabs');
      return launchOptions;
    }
  });
};
```

What we are doing here is we are listening to an event called `'before:browser:launch'` and if the browser that we are using is Chrome, then we want to add the option '--auto-open-devtools-for-tabs'. Now when you open up your Cypress Test Runner, you should also see that DevTools is opened automatically.

## Using cy-api plugin

If you don't want to load your DevTools console automatically, an alternative way is to use the [cy-api](https://github.com/bahmutov/cy-api) plugin. This plugin will render the request and response output to the Test Runner where your application preview would be normally. Using this plugin is very straightforward and only requires couple of changes.

You need to install the plugin by running:

`npm i --D @bahmutov/cy-api`

Then on your `support/index.js` file, add the following line:

`import '@bahmutov/cy-api/support';`

Once the above steps is completed, whenever you make a call to `cy.request()`, this just needs to be replaced with `cy.api()` and that's pretty much it!

```
/// <reference types="cypress" />

describe('Post API', () => {
  it('loads all the posts successfully', () => {
    cy.api({ url: '/posts' }).then((res) => {
      expect(res.status).to.equal(200);
      expect(res.body.length).to.equal(100);
    });
  });

  it('loads the correct amount of posts per user', () => {
    cy.api({ url: '/posts?userId=1' }).its('body').should('have.length', 10);
  });

  it('should add a new post successfully', () => {
    cy.api({
      method: 'POST',
      url: '/posts?userId=1',
      body: {
        userId: 1,
        title: 'foo',
        body: 'var',
      },
    }).then((response) => {
      expect(response.status).to.equal(201);
      expect(response.body.id).to.equal(101);
    });
  });

  it('should delete a post successfully', () => {
    cy.api({ method: 'DELETE', url: '/posts/1' })
      .its('status')
      .should('be.equal', 200);
  });
});
```

You can see on the above code snippet that it's signature is similar to cy.request(). When you run the tests with Cypress Test Runner, you should then see the output in the application preview.

<figure>
  <img src="../../images/cypress-test2.gif" alt="Cypress test runner in action">
</figure>

What's great about this is you can time travel back to your tests as much as you want for debugging. It also outputs the response status code as well as the time it takes for the request to finish.

## Final Thoughts

If you are thinking of using a new tool to test your API and you are already using Cypress to test your UI, why not give Cypress a try for API testing too? By using the same tool, skills are easily transferable since context switching is minimal. We also have more buy-in from developers to do API testing since using Cypress for front end testing is very popular in the development community. However with everything, you should always find the tool that works best for you and your team. 🙂