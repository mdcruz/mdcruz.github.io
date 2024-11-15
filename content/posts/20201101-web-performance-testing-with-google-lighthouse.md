---
author: "Marie Cruz"
title: "Web Performance Testing with Google Lighthouse"
date: "2020-11-01"
tags:
- testing
- tools
- test-automation
- performance-testing
---

Humans now have less attention span. There were alarming studies out there that the average human attention span is now down to 8 seconds from 12 seconds and suggesting that our attention span is now less than a goldfish. Although, this has now been debunked and not entirely true as written in this article, [Busting the attention span myth](https://www.bbc.co.uk/news/health-38896790). When it comes to a website's page speed however, studies and findings such as [Slow websites are silent killers for businesses](https://www.businesswire.com/news/home/20180724005488/en/Slow-Websites-Are-Silent-Killers-for-Businesses), still suggest that slow websites frustrate users and they are highly likely to switch to competitors if the websites that they are using are consistently slow to load.

## Client Side Performance Testing

When we talk about performance, for some of us, we probably think about server or backend performance. In order to address server performance issues, teams make optimisation to database queries or implement load balancing to distribute network traffic to different servers. However, even if the response times for different requests are fast enough, most of the time, the client side is not optimised and contributes to an increase in overall response time especially if it's downloading a lot of scripts or images. I've already mentioned at the start that slow websites frustrate users which therefore drives away business.

The great news is there are so many tools out there to test the performance of your application for both client and server side. I've previously blogged about using [Taurus](https://www.testingwithmarie.com/posts/20200723-using-taurus-for-performance-testing/) and [k6](https://www.testingwithmarie.com/posts/20200824-shifting-performance-testing-to-the-left-with-k6/) though I focused more on doing load testing. Load testing is essential since it gives us insights on how our applications behave under heavy load conditions. However, single user performance testing is as important. Whenever a user performs different user flows, one indicator of a great user experience is the page speed. Everyone wants fast responsive websites so on this post, let's look at how we can use one of the most popular tools for client side performance testing.

## Google Lighthouse

One of the most used tools out there for testing the performance of your website is [Google Lighthouse](https://developers.google.com/web/tools/lighthouse), an open source tool for auditing and improving the quality and performance of your website. It's a popular choice due to its versatility and ability to measure different areas such as web performance, accessibility, search engine optimisation and more. There are different ways that you can use Lighthouse which I'll cover briefly on this post.

The easiest is probably running it via Chrome DevTools since it's built in. Just open up developer tools, select the Lighthouse tab and run the audit. When it's finished, a set of scores will be calculated based on checking different metrics such as first contentful paint, largest contentful paint and time to interact. This article, [Lighthouse Performance Scoring](https://web.dev/performance-scoring/), provides a really good explanation on how the score is calculated and what the different metric means.

If you want more flexibility and advanced usage, you can also install Lighthouse CLI which allows you to run the audits via the command line or programmatically. This requires a version of node to be installed on your machine however. 

To install Lighthouse CLI globally on your local machine and run an audit via the command line, just type:

```
npm install -g lighthouse

// once installed, run:
lighthouse <your url here>
```

The following command is an example running an audit on the airbnb website on desktop mode. Passing the last parameter --view will automatically display the lighthouse audit report.

`lighthouse https://www.airbnb.co.uk/ --emulated-form-factor="desktop" --view`

I highly recommend going over their Github readme file for additional options that you can pass to the lighthouse command. Alternatively, you can also pass the `--help` parameter for more options.

One thing you might notice when using the CLI tool as opposed to running it from Chrome developer tools is that the results you get from Lighthouse CLI (report from the left) might be different from the one from Chrome devtools (report from the right). Don't get fixated by how different these numbers are as the tests are run on different conditions based on what network and CPU throttling are used. 

<figure>
  <img src="../../images/lighthouse.png" alt="Example of performance audit scores using Google Lighthouse CLI and Lighthouse Devtools">
  <figcaption>Performance audits from Lighthouse CLI and Lighthouse Devtools</figcaption>
</figure>

It's better to focus on what improvements you and your team can work on. Google Lighthouse provides a set of suggestions on how to speed up your website and how much estimated savings you can get by implementing these suggestions.

## Running Lighthouse Audits from Cypress

Yes you read that correctly! You can also run Google Lighthouse audits directly from your Cypress tests. One of the things I like about Cypress is the various collections of [Cypress plugins](https://docs.cypress.io/plugins/index.html), backed by the community, which you can use to extend its default behaviour. One of the plugins I recently discovered is called [cypress-audit](https://www.npmjs.com/package/cypress-audit) which allows you to extend Cypress so you can have custom commands to easily test the performance and accessibility of your site using Google Lighthouse and [Pa11y](https://pa11y.org/). 

This has the added benefit of allowing you to easily integrate client side performance testing in your build pipelines earlier on and re-use existing tests to leverage performance testing. I think this plugin would work best on scenarios where you are actively monitoring the scores and make sure that it doesn't go down a certain threshold when your team integrates new features. 

This has the added benefit of allowing you to easily integrate client side performance testing in your build pipelines earlier on and re-use existing tests to leverage performance testing. I think this plugin would work best on scenarios where you are actively monitoring the scores and make sure that it doesn't go down a certain threshold when your team integrates new features. 

I have created this repository [cypress-lighthouse-demo](https://github.com/mdcruz/cypress-lighthouse-demo) so you can have a look at the full code but to get started, simply follow the usage instructions on the [cypress-audit readme file](https://github.com/mfrachet/cypress-audit#preparation).

To run the most basic audit, just use the command `cy.lighthouse()` after visiting a page.

```
describe('Personal Blog', () => {
  it('should run lighthouse performance audits using default thresholds', () => {
    cy.visit('/');
    cy.lighthouse();
  });
}
```

By default, if you don't provide any arguments to the lighthouse command, the test will fail if any of the categories have a reported score of less than 100.

Once the test has finished, you should see a log displaying any categories that were less than the default threshold. By default, this also simulates your page on mobile view.

To override any of the default configuration, we can pass in the thresholds that we want to monitor with the corresponding scores.

```
it('should run lighthouse performance audits using custom thresholds', () => {
  const thresholds = {
    performance: 50,
    accessibility: 80,
    'first-contentful-paint': 2000,
    'largest-contentful-paint': 3000,
    interactive: 2000,
    seo: 60,
    pwa: 50,
  };

  const lighthouseConfig = {
    formFactor: 'desktop',
    screenEmulation: { disabled: true },
  };

  cy.visit('/');
  cy.lighthouse(thresholds, lighthouseConfig);
});
```

By looking at the code snippet above, I created two objects that I pass as arguments on the cy.lighthouse command. One is for the thresholds which contain some custom numbers that I came up with. The second object is additional lighthouse configuration that will allow me to check the lighthouse test on desktop. You can customise it easily to override different configurations. I highly recommend checking out [Lighthouse Emulation](https://github.com/GoogleChrome/lighthouse/blob/master/docs/emulation.md) so you can understand what other properties need to be modified.

## Wrapping up

Google Lighthouse is a powerful tool and as you can see there are several ways of using it.  Having it run directly inside Cypress also makes it easier to monitor client side performance issues. If your team is already using Cypress and looking at new ways to integrate performance testing, then give the cypress-audit plugin a try. Huge thanks to [Marvin Frachet](https://github.com/mfrachet) for creating this plugin! 

You can also check out [Ravi's github repository](https://github.com/vrknetha/cypress-lighthouse) as another example of using the plugin.
