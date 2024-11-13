---
author: "Marie Cruz"
title: "Modern Web testing with Cypress"
date: "2020-02-28"
tags:
- testing
- tools
- test-automation
- cypress
---

The web has evolved.
Finally, testing has too.

This is what you will see when you visit [Cypress](https://www.cypress.io/)' homepage. It promises a fast, easy and reliable way for running User Interface (UI) tests. For so many years, Selenium has been the de facto test automation tool for writing these tests. A lot of companies use it and QA engineers advocate using it as part of their testing strategy. If you have been doing UI automation for quite some time however, you would know by now that developers do not like writing UI tests. It's not because they don't care about quality (they very much do), they just find existing tools hard to use. Selenium is a very mature testing tool however, it's also difficult to setup as you would need to install other libraries with it. Debugging tests with Selenium is also not as straightforward which might put off some developers in contributing to writing the tests as it might slow them down. This is where Cypress comes in. 💜

## Why Cypress is popular?

A quick check on npm trends shows that Cypress is indeed very popular to the JavaScript community. Since the projects that I've been involved lately are all JavaScript related, I have been using Cypress and it's an awesome tool! It's mainly use for UI functional test automation but I have also use it to test APIs, do accessibility checks and visual validations. 

<figure>
  <img src="../../images/cypress-trends.png" alt="Trends chart comparing cypress, webdriverio, nightwatch, and testcafe">
  <figcaption>Cypress trends over six months.</figcaption>
</figure>

Cypress has evolved tremendously from when it was first released, with their latest [release](https://cypress.io/blog/2020/02/06/introducing-firefox-and-edge-support-in-cypress-4-0/?utm_campaign=Docs%20Banner&utm_medium=blog%20post&utm_source=blog&utm_content=text%20link) now supporting Firefox (beta) and Edge browsers. Additionally, [State of JS 2019](https://2019.stateofjs.com/testing/) reported that Cypress is the 2nd most popular testing framework, which shows that a lot of developers and QA engineers are switching to using it. The Cypress community is also very active especially with the introduction of [Cypress Ambassadors](https://www.cypress.io/ambassadors/), of which I am very humbled to be a part of as this would allow me to network with brilliant Developers and QA engineers all over the world and continually learn best practices.

## Is Cypress replacing Selenium?

Cypress is different from Selenium. It's like comparing apples to oranges. Selenium was created to cater the needs of QA engineers who, in most cases, don't have access to the code base and prefer to write E2E automation tests. On the other hand, developers like to write tests that are fast and one of the pain points for them was the brittleness and slowness of writing E2E automation tests. Cypress was created to cater the needs of front end developers to persuade them to write tests apart from unit tests. Front end developers use JavaScript hence this is why Cypress is only a JavaScript testing tool. On the other hand, QA engineers work with other programming languages apart from JavaScript such as Java or C#. 

Some will say that it's just another testing tool that does the same thing. However, it's a wide known fact that testing is hard. Writing E2E tests is even harder. Some developers find that Selenium is not the right tool for them as it's not as straightforward to use it to test asynchronous applications.

However, from hearing other developers' thoughts, they feel that some QA engineers whose project is with JavaScript are still reluctant to switch to Cypress as it lacks some features that Selenium provides and has some permanent and temporary tradeoffs as documented [here](https://docs.cypress.io/guides/references/trade-offs.html#Permanent-trade-offs-1). 

Personally, I think Selenium is here to stay but with the growing popularity of JavaScript over the years and the growing focus of test automation responsibility now shifting to the developers with the help of QA engineers, Cypress is sprinting to the top. 

## Why should you use Cypress?

While Cypress has some tradeoffs, they have a lot more to offer and this blog post aims to convince my fellow QA engineers to learn and use Cypress. 🙂

### Easy installation and setup

Believe me when I say that you can get up and running with Cypress in 5 minutes or less. I won't be doing a full tutorial as there are a lot of awesome tutorials out there which I will link  below. Important thing to note is that as long as you have node and NPM installed, you're all good. All you need is the following command.

`npm install cypress`

Once Cypress is installed, you can run the following command which will do two things. First, it will create a scaffolding structure with recommended files to get your started. Second, this will open the Cypress UI test runner.

`npx cypress open`

As of Cypress 4.0, cross browser functional testing is also now made possible. Unlike with Selenium, you don't need to install any additional drivers to enable cross browser testing. If you have the browser installed in your machine, just select it from the dropdown list and click a test file to run.

<figure>
  <img src="../../images/cypress-ui.png" alt="Cypress test runner showing different browsers">
  <figcaption>Cypress 4.0 introduces support for Firefox and Edge.</figcaption>
</figure>

And that is pretty much it! You can now focus on writing your tests instead as Cypress has made the installation and setup so easy. 

### Cypress UI Test Runner

Cypress comes built in with their unique offering of a [UI Test Runner](https://docs.cypress.io/guides/core-concepts/test-runner.html#Overview) which allows you to interactively see how your tests run on each action. From the UI test runner, you can see information such as how many tests passed or failed and the duration of running these tests. It also displays a preview of your application and how it behaves when being tested. Another nice feature that you can use with the UI test runner is its ability to time travel. What this means is you can easily debug the state of your application whenever you click on an action as Cypress can time travel to different snapshots so you can see your application's. behaviour. You also have direct access to Chrome DevTools and Cypress will print additional useful logs that you and your team might find useful.

<figure>
  <img src="../../images/cypress.gif" alt="Cypress UI Test Runner in action">
  <figcaption>Cypress UI Test Runner in action.</figcaption>
</figure>

### Excellent documentation

The Cypress team has invested a lot of their time to try and make their [documentation](https://docs.cypress.io/) clearer for everyone. Their [GitHub](https://github.com/cypress-io/cypress-example-recipes) repo also shows an extensive list of various examples. As I mentioned above, Cypress has some trade offs but these tradeoffs should not hinder you from trying the tool out as the Cypress team have even detailed specific workarounds in their documentation site on these various limitations.

### Clear error messages

The error messages that Cypress provides are very clear and easy to understand as opposed to other tools where you are shown a long stack trace error. This should then increase the productivity more as you spend less time trying to figure out why your test has failed.

<figure>
  <img src="../../images/cypress-error.png" alt="Cypress error messages">
  <figcaption>Cypress error messages</figcaption>
</figure>

### Awesome plugins
Cypress has a collection of plugins which can boost the way you write your tests. As mentioned above, we use Cypress for other types of testing as well. Looking to make your site more accessible? You can use [cypress-axe](https://github.com/avanslaars/cypress-axe) which would allow you to catch some accessibility errors on an earlier stage. Does your team prefer to use Gherkin BDD syntax in writing the tests? You can use [cypress-cucumber-preprocessor](https://github.com/TheBrainFamily/cypress-cucumber-preprocessor). Want visual testing for your components and site? You can use [cypress-image-snapshot](https://github.com/palmerhq/cypress-image-snapshot) or even better, you can integrate with [Applitools](https://applitools.com/tutorials/cypress.html) to cover cross browser visual validation testing.

You can find the full list of plugins available [here](https://docs.cypress.io/plugins/index.html).

### Encourages Developers to write more tests 💜

A lot of companies are moving to a model where developers are encouraged to test their code more and write automated tests. This is where Cypress promotes a culture where quality is everyone's responsibility and working in silos where a separate QA team writes the tests on a different repository is discouraged. Cypress understands the different pain points for developers and they have created a tool that can fit in everyone's toolbox. Most of the time, QA engineers struggle with striking a balance between manual and test automation. By sharing the responsibility of writing automated tests with developers, this gives you more time to focus on other important activities such as exploratory testing.

### Other excellent sources

If you need more convincing as to why Cypress is an excellent alternative, check out the following links:

- [Should I start with Cypress?](https://glebbahmutov.com/blog/should-i-start-with-cypress/)
- [From zero to hero with Cypress](https://www.cypress.io/blog/2019/08/16/webcast-recording-from-zero-to-hero-with-cypress/)
- [Cypress tutorial for beginners](https://www.valentinog.com/blog/cypress/)
- [Test Automation University: Intro to Cypress](https://testautomationu.applitools.com/cypress-tutorial/)