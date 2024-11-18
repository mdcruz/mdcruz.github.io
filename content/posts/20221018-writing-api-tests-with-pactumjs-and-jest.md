---
author: "Marie Cruz"
title: "Writing API tests with PactumJS and Jest"
date: "2022-10-18"
tags:
- testing
- tools
- test-automation
- api-testing
---

Last month, I attended TestBash UK in person and met the awesome Julia Pottinger for the first time. Julia spoke about [Next Level API Automation](https://www.ministryoftesting.com/dojo/lessons/next-level-api-automation) and she explained important concepts about API testing clearly and also shared various tools to help with API testing. 

There are a variety of tools out there that can help you with API testing. For example, I previously blogged about [how Cypress can help with API testing](https://www.testingwithmarie.com/post/api-testing-with-cypress). Understanding the goal that you are trying to accomplish when it comes to API testing is more important rather than sticking to one tool so it's important to choose the tool that works best for you and your team. 

One of the tools that Julia mentioned during her TestBash talk was [PactumJS](https://pactumjs.github.io/) and I was curious to try it out myself as I haven't used this before. So, in this blog post, I'll explain what PactumJS is, how to get started and how to use it for API test automation.

## What is PactumJS?

According to [PactumJS's documentation](https://pactumjs.github.io/introduction/welcome.html#introduction), it is a next generation, free and open-source REST API automation testing tool for all levels in a [Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) that promises a productive and enjoyable experience when it comes to backend testing. 

This library took inspiration from two existing tools:

- [Frisby](https://github.com/vlucas/frisby) - an API testing tool built on top of Jest.
- [Pact](https://docs.pact.io/) - a tool for contract testing which I previously blogged before ([Contract Testing with Pact.js and Jest](https://www.testingwithmarie.com/post/contract-testing-with-pact-js-and-jest)).

## Use cases for PactumJS

Apart from API testing, PactumJS seems to also be used for the following uses cases:

- Component Testing
- Contract Testing
- Integration Testing
- E2E Testing
- Mock Server

Note that for the above use cases, it's all from an API perspective so E2E testing will not make use of a UI tool.

## Installing PactumJS 

To get started with PactumJS, you need to have Node installed then simply install the package via NPM. 

```
# install pactum
npm install -D pactum

# install a test runner
npm install -D jest 
```

PactumJS is not a test runner so you also need to install test runners such as jest, mocha or even cucumber. Feel free to use any test runner that you want but for this blog post, I'll be using jest.

## Writing tests with PactumJS

To start, let's create a simple test that checks if it's returning the correct headers for a given API. I'll be using https://jsonplaceholder.typicode.com/todos as my test API.


To do this, you need the following steps:

1. Initiate a single request to the server. In PactumJS, you can do that by using the `spec` method.
1. Make a HTTP GET request by using the `get` method.
1. Once the request is made, you need to validate the response and use assertions such as `expectHeader` to verify that the header is correct. 

Translating the steps to PactumJS, you get the following code snippet below:

```
const { spec } = require('pactum')

describe('Todo API', () => {
  it('should return the correct headers', async () => {
    await spec()
      .get('http://jsonplaceholder.typicode.com/todos')
      .expectHeader('content-type', 'application/json; charset=utf-8')
  })
})
```

Next, I will demonstrate how to do a POST request. Let's say you want to test that you can create a new todo item successfully.

To do this, you can do the following steps:

1. Initiate a single request to the server.
1. Make a POST request by using the `post` method.
1. Include the JSON body that you need to create a new todo item by using the `withJSON` method. For this example, you need to set the title for your todo, the actual body and a user ID.
1. Validate the response and use assertions such as `expectStatus` to verify the status code and also `expectJsonLike` to verify a partial JSON object.

```
it('should create a new todo item', async () => {
  await spec()
    .post('http://jsonplaceholder.typicode.com/todos')
    .withJson({
      'title': 'Write blog post',
      'body': 'Write blog post about PactumJS',
      'userId': 1
    })
    .expectStatus(201)
    .expectJsonLike('title', 'Write blog post')

  // or you can also do a strict comparison
  // .expectBody({
  //   'title': 'Write blog post',
  //   'body': 'Write blog post about PactumJS',
  //   'userId': 1,
  //   'id': 201
  // })
})
```

If you want a stricter assertion, rather than using `expectJsonLike`, you can also use `expectBody` and provide the entire JSON response body.

The last example that I want to demonstrate is deleting a todo item which is quite simple to do. To do this with PactumJS, you just need to use the `delete` method, pass in the API call and validate the status code.

```
it('should delete a todo item', async () => {
  await spec()
    .delete('https://jsonplaceholder.typicode.com/todos/1')
    .expectStatus(200)
})
```

## Wrap up

As you can see from the above examples, PactumJS is an easy to use tool and provides a nice library to test your APIs. The methods are self explanatory and descriptive enough to guide you when it comes to writing your tests. I highly recommend going over their [API documentation](https://pactumjs.github.io/api/api/table-of-contents.html) for comprehensive information on the different methods that they have.

The examples above only demonstrate API testing. I'm curious to see how it works with the other use cases mentioned above so I'll try and cover them in future blog posts!

All code examples can be found on this Github repository - [pactum-js-demo](https://github.com/mdcruz/pactum-js-demo).

