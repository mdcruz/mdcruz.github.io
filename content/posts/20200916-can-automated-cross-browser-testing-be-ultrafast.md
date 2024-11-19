---
author: "Marie Cruz"
title: "Can Automated Cross Browser Testing Be Ultrafast?"
date: "2020-09-16"
tags:
- testing
- tools
- test-automation
- cypress
- applitools
- applitools-blog
---

This post was originally published on the [Applitools](https://applitools.com/) blog which you can view [here](https://applitools.com/blog/can-automated-cross-browser-testing-be-ultrafast/).

Early January this year, Applitools announced the results of their [Visual AI Rockstar Hackathon winners](https://applitools.com/blog/hackathon-winners/) of which I am lucky to be included as one of their silver winners. I blogged about my experience as to how I approached the hackathon and my honest feedback as to why I think it’s modernising the way we do test automation which you can find in this post – [Applitools: Modernising the way we do test automation](https://www.testingwithmarie.com/posts/20200801-modern-cross-browser-testing-with-cypress-and-applitools/).

6 months in and they announced another hackathon but this time the focus is on cross browser testing via Ultrafast Grid and comparing it with traditional solutions such as running the same functional tests on different browsers and viewports locally. I participated in the hackathon again and ended up as one of their gold winners this time, which I’m extremely pleased, because not only did I win one of their amazing prizes but also, I improved my technical skill and learned a lot about the true cost of cross browser testing.

## Why Automated Cross Browser Testing?

First, let’s talk about why cross browser testing? Why another hackathon?

If you’re like me and have been testing web applications for some time, you’ll know that cross browser testing is a pain and time consuming to test. There is no way you can test 100% cross browser coverage, unless you have a lot of time and want to devote all your testing efforts in cross browser testing alone plus don’t forget that you also need to check on other viewports. In time, it gets really boring and tedious. This is why it’s a great idea to automate cross browser tests as much as possible so we can focus on other areas where we are also needed such as exploratory and accessibility testing.

Nowadays, there are not a lot of functional differences between different browsers. The code that gets deployed is mostly similar on any platform but the way the code is rendered visually exposes differences that we need to catch. Rather than doing cross browser functional testing where we are testing the functionality across different browsers, a better alternative is to do cross browser visual testing instead where we validate the look of our pages instead because this is what our users see.

The problem is automated cross browser testing, whether it’s functional or visual, can still take a considerable amount of time to set up because you need to create an automation framework that can scale and easily be maintained. This can be quite a challenge for testers who are considerably new in test automation.

The purpose of this hackathon was to show how easy and how fast cross browser visual testing can be if you’re using modern tools such as Applitools. It’s also to highlight that existing testing tools are great for doing cross browser functional testing but not so great with cross browser visual testing which I’ll expand on later.

## The Hackathon Experience: Cypress

The hackathon was split into automating three different scenarios for a sample e-commerce website called [AppliFashion](https://demo.applitools.com/gridHackathonV1.html). The scenarios should be automated using any testing tool of your choice and also with the same testing tool but with using Applitools alongside. The automated tests will then be executed on two versions of the website – version 1, which is assumed to be bug free, and version 2, which has new bugs added in. On version 2, you have to update the automated tests to fix the bugs introduced and then compare the effort of doing this with the traditional testing tool that you have chosen as opposed to using Applitools.

I decided to use Cypress as my testing tool and while it’s a great tool for automating the browser, I spent 5.5 hours doing cross browser visual testing with this approach and still felt that I missed a lot of visual bugs. Let’s look at this in more detail.

- Installing Cypress: **5 mins**
- Writing tests for version 1: **2 hours**
- Test maintenance for version 2 and finding bugs: **1 hour**
- Test reporting and project refactoring: **2 hours**
- Documentation: **30 mins**
- **Total Time: 5 hrs, 35 mins**

Writing the tests took quite some time. I needed to verify a lot of the elements and get their selectors so I could assert that they were visible on the page. Some of the elements were hidden if you are on different viewports so these had to be reflected on the tests. You can find an example code on how I handled this on my [applitools ultrafast grid github repo](https://github.com/mdcruz/applitools-ufg-hackathon/blob/master/TraditionalTestsV1/cypress/integration/task3.spec.js). The test execution time was also slightly longer because I had more viewports (desktop, tablet, mobile) and browsers (Chrome and Firefox) to cover locally.

When it was time to run the same test on version 2, I had to make some adjustments to my tests and log the bugs that my automated tests found. This took me an hour because I had to update the selectors to fix my tests but also, I wasn’t confident that my tests found all the visual bugs on version 2. I had to find some of the bugs manually since verifying CSS changes was difficult for me with just using Cypress alone.

When it came to test reporting and project refactoring, I invested 2 hours on this task. As I knew from experience, good reporting helps everyone make sense of test data. I wanted to integrate Mochawesome reporter so I could present the test results nicely to the hackathon judges. I wrote a tutorial on how to do this which you can find in this post – [Test Reporting with Cypress and Mochawesome](https://www.testingwithmarie.com/posts/20200606-test-reporting-with-cypress-and-mochawesome/). I also started noticing that my test code was getting a lot of duplication so I did some refactoring to clean up my automation framework.

## The Hackathon Experience: Cypress with Applitools

Now let’s look at how long it took me to do cross browser visual testing with Cypress and Applitools.

- Install Applitools Cypress SDK: **2 mins**
- Setup project structure with Cypress and Applitools: **10 mins**
- Writing tests for version 1: **20 mins**
- Running tests for version 2: **5 mins**
- Bug reporting with Applitools: **25 mins**
- Documentation: **10 mins**
- **Total time: 1 hr, 12 min**

In total, I spent around just over one hour writing the tests for both version 1 and version 2 when using Cypress with Applitools. The time difference was massive! There were a few visual bugs that I missed that Applitools caught but even if this was the case, I didn’t have to rewrite my tests at all. All the adjustments were done on the Applitools Dashboard directly and marking the bugs through its annotation feature.

Writing the tests was considerably faster. As opposed to verifying individual selectors and checking that it’s visible, I took a screenshot of the whole page which is a better approach for visual validation.

```
it('should display Product details page correctly', () => {
  cy.eyesOpen({
    testName: 'Task 3',
    stepName: 'Product Details Test',
  });
  cy.visit(Cypress.env('PRODUCT_DETAILS_URL'));
  cy.eyesCheckWindow();
  cy.eyesClose();
  });
```

The code snippet above is simpler and will catch more visual bugs with less or even no test code maintenance.

So you might be wondering from the above code, how did I handle the cross browser capabilities? This was easily achieved by creating a file called `applitools.config.js`  on the root of your project and specifying the list of browsers that you want your tests to execute on. By utilising Ultrafast Grid and setting my concurrency to 10, I was able to run the tests quicker too.

```
module.exports = {
  appName: 'AppliFashion',
  batchName: 'UFG Hackathon',
  browser: [
    { deviceName: 'iPhone X', screenOrientation: 'portrait' },
    { width: 1200, height: 700, name: 'chrome' },
    { width: 1200, height: 700, name: 'firefox' },
    { width: 1200, height: 700, name: 'edgechromium' },
    { width: 768, height: 700, name: 'chrome' },
    { width: 768, height: 700, name: 'firefox' },
    { width: 768, height: 700, name: 'edgechromium' },
  ],
  concurrency: 10,
};
```

## The Impact of Visual Cross Browser Testing for Testers

Overall, this was another excellent hackathon from Applitools and showed that cross browser testing can be easy and fast. I’ve mentioned this in the past already that one of the trends that I’m seeing is more and more testing tools are becoming user friendly and if you are new to test automation, this is great news!

Also, from my experience, the production bugs that get missed most frequently are visual bugs. A test that hasn’t loaded any of its CSS files can still work functionally and your automated functional test will still pass. Rather than doing cross browser functional testing, it’s better to do cross browser visual testing to get maximum value.

Finally, the massive time saving that it provides means that we, as testers, have more time to explore the areas that automated tests can’t catch and that is a big win.

For more information:

- Read the white paper with [Empirical Data For the Cross Browser Hackathon](https://applitools.com/modern-cross-browser-testing-report/)
- Sign up for a [free Applitools account](https://www.applitools.com/free/).
- [Request an Applitools demo](https://www.applitools.com/request-demo/)

