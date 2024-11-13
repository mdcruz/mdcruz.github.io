---
author: "Marie Cruz"
title: "Adding more stability to your Cypress Tests"
date: "2020-05-17"
tags:
- testing
- tools
- test-automation
- cypress
---

For test automation to be effective, it needs to execute the same behaviour it is trying to test multiple number of times. If a test is failing consistently then that means something has broken your tests but if a test is failing randomly, then it's a flaky test and needs to be resolved. While we all know the benefits of test automation, your automated tests will not give you 100% benefit if the tests are flaky. Therefore, it's reasonable to isolate any flaky tests you have and fix them immediately. If not, then you slowly start to lose the value these tests bring. I have experienced this in the past where our test will fail randomly and then the team will not bother checking the results because the test will pass when you run it again anyway. But then, this also meant that when an actual test failed and the results were not checked, you then have the risk of teams ignoring the results and deploying the changes to production. I definitely struggle with this one from time to time so will share some of the tips I've learned from the past which can help in making your tests stable. So if you are like me and fighting with flaky tests, this post is for you! While this blog post is specific to Cypress, the tips below can also be used to other test automation frameworks.

## Tip #1: Use resilient selectors

I highly recommend this [blog post](https://kentcdodds.com/blog/making-your-ui-tests-resilient-to-change) by Kent C. Dodds if you haven't read it yet. Here, Kent talks about why we need to make use of data-testid attributes to make our element strongly identifiable and resilient to changes. By using data-testid attribute, you know that this selector will not be affected by any CSS or JavaScript changes in your code. For example, let's say we have a Button component and the selector for it is dynamic, we can easily add a custom data-testid attribute to make it unique. This is what one of our teams have done at News and they've proven to be effective.

`<SubmitButton data-testid="submit-button" />`

We can then use it in our Cypress tests like so:

`cy.get('[data-testid="submit-button"]');`

Alternatively, you can replace data-testid with data-cy which is what is recommended by Cypress team.

## Tip #2: Don't use unnecessary waits

This used to be something that I add a lot when I write automated tests. Depending on the test framework, I will add thread.sleep(), or browser.pause() or even cy.wait() and voila my tests will pass. I learned the hard way that these are bad practices so when you are tempted to add waits, don't do it! Cypress will actually automatically retry and wait for your element to be visible. If you need to actually do a wait, try changing the default timeout property of your command instead.

For example, rather than doing the following where you are explicitly waiting for 10 seconds before you get the element that you need.

```
cy.wait(10000);
cy.get('.nextArrow');
```

You can use the following command instead and this will wait for a maximum 10 seconds. Cypress will keep retrying and resolve as soon as possible, meaning if the element is found before 10 seconds, it will continue with executing the rest of the commands.

`cy.get('.nextArrow', { timeout: 10000});`

## Tip #3: Wait for a network request to finish

Another approach that I find useful is waiting on a XHR request which your test is dependent on. This is particularly helpful if you are not mocking requests for whatever reason. As an example, let's say we have a test which verifies the results of an API request. Sometimes, our API providers will take a longer time to complete. When this happens, our test can fail because it doesn't have the correct state it needs. I wrote a sample image search app before where it was using a third party API to display some default search results when you visit the page. When I run my tests, it was failing intermittently because the API request was taking longer than usual and my image search app was not displaying anything on the screen. Rather than hardcoding a wait value, what you can do instead is to wait for this particular request to finish before you can proceed with the rest of your tests.

This can be achieved with the help of a couple of Cypress's built in command (`cy.server` and `cy.route`).

```
cy.server();
cy.route('photos?page=1).as('defaultSearchResults');

cy.visit('/').wait('@defaultSearchResults');
cy.get('[data-test-id="image-gallery"]');
```

What we are doing above is we are intercepting the request `photos?page=1` and aliasing it as defaultSearchResults. Another thing to note is that we are not mocking the request here. After we visit the page, Cypress will automatically wait for this request to complete. You can also use tip #2 in this case and override the default request and response timeout if needed as detailed [here](https://docs.cypress.io/api/commands/wait.html#Timeouts).

## Tip #4: Mock third party dependencies if possible

If you are testing a complex website which calls a lot of third party requests, this can slow down your tests and sometimes results in Cypress visiting the page to fail. What I have done on one project that we are helping with is to mock the third party dependencies so we can just focus on testing our own features. As an example, we have a cookie banner that pops up every time we visit our page. While we already have a test which accepts the cookie banner, we don't need to do this every time on our other tests. This is where mocking can help you.

```
Cypress.commands.add('mockConsentRequest', () => {
  cy.server();
  cy.route('**/consent/v2/**/*', {
    consentedToAny: true,
  });
});
```

With the above, we have created a Cypress custom command which mocks matching request to `consent/v2` and we are setting the `consentedToAny` property to true which will simulate that the cookie banner has been accepted. 

Another useful benefit of mocking is that if the API that you are integrating with is not stable and you still want to test that the front end part of your application behaves as expected, you can mock this third party API to return a specific test data.

```
cy.fixture('unsplash').as('unsplashData');
cy.route('search/photos?query=mock+', '@unsplashData');
```

With the above, my test data is loaded in as a fixture and I am telling Cypress to mock the endpoint which matches `search/photos?query=mock+`. When this endpoint is found, it will be intercepted to return the test data fixture instead.

## Tip #5: Use Docker

As explained on my previous [post](https://www.testingwithmarie.com/posts/20200504-using-docker-to-run-your-cypress-tests/), we were seeing test flakiness in our visual tests using Cypress. There were a lot of mismatches on our visual tests because the baseline image was generated on our local machine but the test images are generated on our CI server which is running on a different system. By using Docker, we are generating both the baseline and test images on the same configurations which helped in reducing the unwanted mismatches and only reported the actual valid differences.

## Tip #6: Delete them if they are not providing value

This brings me to my final tip for this post. When you do get a flaky test and you can't fixed them immediately, remove them from your build pipeline and identify what the root cause of the issue is. If this is something that your team won't be able to address and the test is still flaky after a month or so, ask yourself if this test is actually giving your team some added value. If the answer to that is no, just delete them! Do some purging! It's alright as long as you get overall agreement within your team. If the answer to that is yes, then do invest some of your time to fix it and definitely ask help from your team when needed.

