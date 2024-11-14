---
author: "Marie Cruz"
title: "Modern Cross Browser Testing with Cypress and Applitools"
date: "2020-08-01"
tags:
- testing
- tools
- test-automation
- cypress
- applitools
---

This post was originally published on the [Applitools](https://applitools.com/) blog which you can view [here](https://applitools.com/blog/modern-cross-browser-testing-cypress/).

With the recent announcement of Applitools Ultrafast Grid Hackathon winners, I luckily made it as one of the Gold winners! The idea behind this hackathon was to write cross browser and cross device tests against a real world application with using traditional testing tools as well as using the Applitools Ultrafast Grid. 

The hackathon consists of 3 tasks which need to be automated using the traditional testing tools and with using Applitools. The tests will be executed on 2 versions of the real world application - V1 (which is assumed to be bug free) and V2 (which has new bugs added in). One of our task is to update the tests to fix the bugs introduced on V2 and we then need to compare the effort of doing this with the traditional testing tool that we have chosen as opposed to using Applitools. 

The list of all winners can be found [here](https://applitools.com/hackathon/) and more details from the hackathon and how the prizes is structured can be found [here](https://applitools.com/blog/ultrafast-cross-browser-hackathon/) but basically, out of almost 2000 submissions, I managed to make it to the top 20 and was awarded one of the gold winners so thank you Applitools! Well done and Congratulations as well to all the winners! 🥳

If you are interested to see my solution to the hackathon, you can view my repo [here](https://github.com/mdcruz/applitools-ufg-hackathon).

Anyway, since I use Cypress with Applitools for the hackathon, on this blog post, I'll show you how to get started with doing modern cross browser testing with these two amazing tools. 🙂

--

This is an accompaniment post to a Cypress lunch and learn series that I've done previously. An example github repo which contains the different code examples can be found [here](https://github.com/mdcruz/react-sample-apps/tree/master/image-search).

Cypress, among other things, validates the structure of your DOM. It verifies that a button is visible or that it has the correct text. But what about the look and styling of our app? How can we test that our application looks good visually? We can use Cypress to verify that it has the correct CSS properties but then our code would look very long and complex. It will be a guarantee that engineers will avoid maintaining this test. 🙈

## Visual Testing

This is why we need visual testing as part of our test strategy. With visual testing, we are validating what the user sees on different browsers/viewports/mobile devices. However, it's very time consuming when you do it manually. 

Imagine if someone told you that you have to test the below image manually. 

<figure>
  <img src="../../images/image-differences.png" alt="Image puzzle with differences">
  <figcaption>source: http://www.puzzlesandriddles.com/SpotTheDifference14.html</figcaption>
</figure>

There are 30 differences. You can probably find them after quite some time. But then this provides a really slow feedback loop. The obvious solution would be of course to automate this!

Now, automated visual testing is not new. There are so many tools out there which can help you with visual testing. These tools use pixel-by-pixel comparison to compare the two images. The idea is you have a baseline image which is your source of truth. Every time there is a code change and our automated tests is run, a test image will be taken. The visual testing tool will then compare this test image with the baseline image and then check the differences. At the end, it will report to us whether our test passed or failed.

## Problem with pixel-by-pixel comparison

The problem with pixel-by-pixel comparison though is it's very sensitive even with small changes. Even if you have a 1px difference, your test will fail even though on the human eye, the two images look exactly similar.

<figure>
  <img src="../../images/the-sun.png" alt="The Sun mobile website">
</figure>

You also get the issue that if you try to run these tests on your build pipelines, you might see a lot of pixel differences especially if the base image is generated locally such as the image above. Looking at the test image above, if you ignore the mismatch image in the middle and compare the two images on left and the right, some of the changes that were reported look ok.  But because these images were taken on different machine setups , the tool has reported a lot of pixel differences. You can use Docker to solve this and generate the base image using the same configuration as the test image, but from personal experience, I still get flakiness with using pixel-by-pixel comparison tools.

<figure>
  <img src="../../images/the-sun2.png" alt="The Sun mobile website">
</figure>

Also, what if you have dynamic data? In the test image above, we have a change of data here but the overall layout is similar. You can probably set the mismatch threshold to be slightly higher so your tests will fail only if it reach the threshold that you defined. The problem with this though is that you might miss actual visual bugs.

## Cross Browser Visual Validation

Most of the existing open source tools for visual testing only runs on 1 or 2 browsers.  For example, one of the tools that we were using before, called BackstopJS,  which is a popular visual testing framework, only runs visual tests on Chrome headlessly.  AyeSpy, which is a tool that was actually created by one of the teams here at News UK, is another visual testing tool, which hooks into your Selenium Grid to run your visual tests on different browsers. But still, it's a bit limited. If you are using the Selenium Docker images, they only have images for Chrome and Firefox. What if you want to run your visual tests on Safari or Internet Explorer?  You can definitely verify this yourself, but again as mentioned, it's time consuming.

How can we solve these different visual testing issues?

## Applitools

This is where Applitools comes in. Unlike existing visual tools, Applitools uses AI comparison algorithms so images are compared just like a human would. It was founded on 2013 and pretty much integrates with almost all testing frameworks out there. You name it - Selenium, Cypress, Appium, WebdriverIO, Espresso, XCUITest, even Storybook! With Applitools, you can validate visual differences on a wide range of browsers and viewports. By using different comparison algorithms (exact, strict, layout and content), you have different options to compare images and can cater different scenarios such as dynamic data or animations.

Rather than taking a screenshot, Applitools will extract a snapshot of your DOM. This is one of the reasons why visual tests are fast to run in Applitools. Once the DOM snapshots have been uploaded to the Applitools Cloud, the Applitools Visual Grid, which offers users a way to run tests in multiple browsers and viewports simultaneously, will then generate the screenshot and does the image comparison. 

## Getting Started

To get you started, you need to install the following package to your project. This is specific to Cypress so you would need to install the correct package depending on your test framework of choice.

`npm i --D @applitools/eyes-cypress`

Once this has been installed, you need to configure the plugin and the easiest way to do this is to run the following command on your terminal

`npx eyes-setup`

This will automatically add the necessary imports needed to get Applitools working in your Cypress project. 

## Writing our first test with just Cypress

Let's start doing some coding and add some validations on a sample react image app that I created a while back. It is a simple image gallery which uses unsplash API for the backend.

<figure>
  <img src="../../images/reactsplash.gif" alt="ReactSplash website">
</figure>

Our Cypress test can look like the following code snippet. Keep in mind, this only asserts the application to some extent. I can add more code to verify that it has the correct CSS attributes but I don't want to make the code too lengthy.

```
it('should have the correct values and styling', () => {
  cy.get('h1.header').should('have.text', 'ReactSplash');
  cy.get('h2.sub.header').should(
    'have.text',
    'The internet’s source of freely usable images. Well, powered by unsplash API...'
  );
  cy.get('[data-testid="search-input"]').should(
    'have.attr',
    'placeholder',
    'Search free high-resolution photos'
  );
  cy.get('.search.icon');
});
```

## Writing our first test with Cypress and Applitools

Now, let's look at how we can write the test using Cypress and Applitools.

```
it('should look ok visually', () => {
  cy.eyesOpen({
    appName: 'ReactSplash',
    batchName: 'Image search',
    browser: [
      { width: 1024, height: 768 },
    ],
  });

  cy.eyesCheckWindow();
  cy.eyesClose();
});
```

Applitools provide the following commands to Cypress as a minimum setup. `cy.eyesOpen` initiates the Cypress eyes SDK and we pass some arguments such as our app name, batch name and the browser size (defaults to Chrome). The command `cy.eyesCheckWindow` will take a DOM snapshot so every call to this command means a DOM snapshot will be generated. You can call this every time you do an action such as visiting your page under test or clicking buttons, and dropdown menus. Finally, once you are finished, you just call `cy.eyesClose`. To know more about the Cypress eyes SDK, please visit their documentation [here](https://github.com/applitools/eyes.sdk.javascript1/tree/master/packages/eyes-cypress) to find more information.

In order to run this in Applitools, you need to export an API key which is detailed on this [article](https://applitools.com/docs/topics/overview/obtain-api-key.html). Once you have the API key, on your terminal you need to run:

```
export APPLITOOLS_API_KEY=<your_key>
npx cypress open
```

Once the test is finished, if you go to the Applitools dashboard, you should see your test being run. The first time you run it, there will be no baseline image then when you reran the test and everything looks good, you should see the following.

<figure>
  <img src="../../images/applitools.png" alt="Applitools visual UI">
  <figcaption>Both baseline and test image are compared 🎉</figcaption>
</figure>

## Handling Dynamic Data

Since we are using the unsplash API, we don't have control as to what data gets returned. When we refresh the app, we might get different results. As an example, the request that I am making to unsplash will get the popular photos on a given day. If I reran my test again tomorrow, then the images will be different like the one shown below.

<figure>
  <img src="../../images/applitools2.png" alt="Applitools visual UI showing image differences">
  <figcaption>Layout looks similar but our data has changed</figcaption>
</figure>

The good thing is we can apply a layout region so the actual data will be ignored or we can also set the match level as Layout in our test which we can preview on the dashboard. If the layout of our image gallery has changed, Applitools will report it as an issue. 

<figure>
  <img src="../../images/applitools.gif" alt="Applitools visual UI">
  <figcaption>Using Layout region to ignore data differences</figcaption>
</figure>

## Making code changes to our application

Now, let's create some changes in our application (code references found [here](https://github.com/mdcruz/react-sample-apps/tree/feat/new-app-changes)) and introduce the following:

- Update the background colour of the search bar (bug)
- Add a new footer component (new feature)
- Remove camera icon on the subtitle (bug)

If we run the test where we only use Cypress, how many of these changes do you think your test will catch? Will it catch that there is a new footer component? How about updating the background colour of the search bar? How about the missing header icon? Probably not because we didn't write any assertions for it. Now, let's rerun the test written in Cypress and Applitools.

<figure>
  <img src="../../images/applitools2.gif" alt="Applitools visual UI">
  <figcaption>Raise issues directly on Applitools dashboard</figcaption>
</figure>

Looking at the image above, it caught all the changes and we didn't had to update our test since all the maintenance is done on the Applitools side. Any issues can be raised directly on the dashboard and you can also configure it to integrate to your JIRA projects.

## Cross Browser Visual Validation

To run the same test on different browsers, you just need to specify the browser options on your Applitools configuration. I've refactored the code a bit and created a file called `applitools.config.js` and moved some of the setup we added initially in our `cy.eyesOpen` in this class.

```
module.exports = {
  appName: 'ReactSplash',
  batchName: 'Image search',
  browser: [
    { width: 1024, height: 768, name: 'chrome' },
    { width: 1024, height: 768, name: 'safari' },
    { deviceName: 'iPhone 5/SE' },
    { deviceName: 'iPad' },
  ]
};
```

Just simply reran your test and check the results in the Applitools dashboard.

## Final Thoughts

This is just an introductory post on how you can use Applitools so if you want to know more about its other features, check out the following resources:

- [Applitools Master Class Webinar](https://www.youtube.com/watch?v=sbVOXvkqM90)
- [Applying Match Levels](https://applitools.com/docs/common/cmn-eyes-match-levels.html)
- [Applying Match Regions](https://applitools.com/docs/topics/test-manager/viewers/tm-viewer-step-editor.html#Add2)
- [Test Automation University - different courses on using Applitools with other frameworks](https://testautomationu.applitools.com/)

While open source pixel by pixel comparison tools can help you get started with visual testing, using Applitools can modernised the way you do testing. As always, do a thorough analysis of a tool to see if it will meet your testing needs. 🙂