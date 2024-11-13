---
author: "Marie Cruz"
title: "Mocking XHR requests with Cypress"
date: "2020-03-10"
tags:
- testing
- tools
- test-automation
---

In this blog post, we'll look at how you can easily mock data from third party APIs using Cypress. Mocking has some advantages such as faster running of tests and enables your front end tests to be decoupled from any backend API. 

The sample app that I will use for this tutorial is a react test app that I created recently from Stephen Grider's [Modern React and Redux course](https://www.udemy.com/course/react-redux/). It's a simple image gallery search which uses unsplash API as its backend.

<img src="../../images/react-splash.gif" alt="ReactSplash user interface">

## Cypress setup

To get started, let's run the following commands to install Cypress and set up initial structure.

```
npm i --save-dev cypress
npx cypress open
```

On the `cypress/integration` folder, delete the example spec files and add a new file called `image-search.spec.js`. Let's first write our tests without mocking any XHR requests. 

```
describe('Image search', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.get('[data-testid="search-input"]').as('searchInput');
    cy.get('[data-test-id="image-gallery"] img').as('imageGallery');
  });

  it('should not return any images when search term is invalid', () => {
    cy.get('@searchInput').type('jehrjehrjehrjkerae {enter}');
    cy.contains('No content found').should('be.visible');
    cy.get('@imageGallery').should('not.be.visible');
  });

  it('should return images when search term is valid', () => {
    cy.get('@searchInput').type('pancakes {enter}');
    cy.get('@imageGallery')
      .should('be.visible')
      .and('have.length', 10);
  });
});
```

The above spec file contains two scenarios. The first scenario verifies that images are not returned and the text "No content found" is displayed on our application if unsplash API does not return any available data while the second scenario verifies that the image gallery component is rendered and returns 10 images. However, with unsplash API, if you are on development mode, you are only limited to make 50 requests per hour to their API. Imagine if we hit that limit and we run our tests, the request call will fail which in turn will fail our tests.

## Mocking XHR requests with Cypress

As we are only interested that our image component renders as expected, let's now write a test which mocks the XHR request to unsplash.

```
/// <reference types="Cypress" />

describe('Image search', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.get('[data-testid="search-input"]').as('searchInput');
    cy.get('[data-test-id="image-gallery"] img').as('imageGallery');
  });

  it('should return mock data', () => {
    cy.fixture('unsplash').as('unsplashData');
    cy.server();
    cy.route('search/photos?query=mock+', '@unsplashData');

    cy.get('@searchInput').type('mock {enter}');
    cy.get('@imageGallery')
      .should('be.visible')
      .and('have.attr', 'description', 'This is a mock data')
      .and('have.length', 10);
  });
});
```

Our `beforeEach` hook is still the same code. Notice though that before I typed a search term, I made use of additional commands from Cypress which I will explain in detail below:

1. `cy.fixture('unsplash')` loads my mock data which I've called `unsplash.json` and is saved on my `cypress/fixtures` folder. This json file is basically an array of objects which contains my mock response for the unsplash API. I've also aliased this fixture as `@unsplashData` so I can reference this later.
1. `cy.server()`  tells Cypress that we will be creating a mock server and will intercept any incoming network requests.
1. `cy.route('search/photos?query=mock+', '@unsplashData')` tells Cypress that for any route that matches our first parameter, we want this to return the `@unsplashData`  fixtures.

To make sure that we have mocked the XHR request successfully, I've written an assertion which checks that the image description is set to "This is a mock data".

From the image below, you can also see that it has loaded our test fixtures correctly and our test passed. Notice as well that when we make a request to the unsplash API, it's marked with (XHR STUB) which is another visual indicator that we have successfully intercepted the network request.

<figure>
  <img src="../../images/react-splash-mocked.gif" alt="Mocked version of ReactSplash running on Cypress">
  <figcaption>Mocking XHR requests with Cypress</figcaption>
</figure>

Mocking data with Cypress is easy but this solution would only work for XHR requests. Requests using the Fetch API and other network requests like script tags are not currently intercepted. There's a lot of details and workarounds on how to mock these requests as reference here https://github.com/cypress-io/cypress/issues/95. 

For the complete code reference, [here](https://github.com/mdcruz/react-sample-apps/tree/master/image-search) is the link to my GitHub repo. Hope this helps! 🙂