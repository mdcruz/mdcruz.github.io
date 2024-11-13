---
author: "Marie Cruz"
title: "Applitools: Modernising the way we do Test Automation"
date: "2019-11-19"
tags:
- testing
- tools
- test automation
---

Recently, I participated in a coding hackathon called [Applitools Visual AI Rockstar Hackathon](https://applitools.com/hackathon). The idea is to write two set of tests for this [demo app](https://demo.applitools.com/hackathon.html) with the first set using existing automation frameworks such as Selenium, WebdriverIO and Cypress and the second set of tests using Applitools AI SDK. By combining Applitools with your functional testing framework, the goal of this hackathon was to highlight the efficiency of combining visual validations with functional tests.

Firstly, what is Applitools? Applitools is a software testing platform which uses visual AI to compare images like a human eyes would.

Traditionally, when you write an automated tests for a Login application and you want to verify that it has the correct fields, labels and buttons, you will need to write assertions for each of the element that you want to verify.

```
describe("Login Page", () => {
  it("should display all fields and labels as expected", () => {
    cy.visit("/hackathon.html")
    cy.get(".logo-w a img").should("be.visible");
    cy.get(".auth-header").should("contain", "Login Form");
    cy.get("#username").should("be.visible");
    cy.get(".os-icon-user-male-circle").should("be.visible");
    cy.get("#password").should("be.visible");
    cy.get(".os-icon-fingerprint").should("be.visible");
    cy.get("#log-in").should("be.visible");
    cy.get(".form-check-label")
      .should("be.visible")
      .should("have.text", "Remember Me");
    cy.get('img[src="img/social-icons/twitter.png"]').should("be.visible");
    cy.get('img[src="img/social-icons/facebook.png"]').should("be.visible");
    cy.get('img[src="img/social-icons/linkedin.png"]').should("be.visible");
  });
});
```

Looking at the code snippet above, the only browser action that I really did was to visit the login page. The rest are all assertions. The problem with this is that if any of the selectors changed, your automated test will also fail. Also, if new changes are introduced (e.g. social media links removed from the login page), your automated test will also fail! The level of maintaining this test is very high which can prevent you from doing other important QA related stuff. This was definitely true when I did the hackathon. While writing this test, I kept thinking to myself "how many more selectors do I need to copy and paste so I can get a good testing coverage 😓?". On top of it, I had to make a lot of changes on my code when I ran the same test on the [app's 2nd version](https://demo.applitools.com/hackathonV2.html). Overall, it took me 2.5 hours to complete the tests for all the testing scenarios. 

On the other hand, with Applitools, believe it or not, it only took me 5 mins to complete all the tests 😲. Most of the refactoring I did was to basically delete all the assertions I made on the previous test and then replace it with this gem `cy.eyesCheckWindow()` which is the command you tell to Applitools to take a screenshot of the page. The same test above had been converted to the code snippet below.

```
describe("Login Page", () => {
  it("it should display all fields and labels as expected", () => {
    cy.eyesOpen({ appName: "ACME Demo App", batchName: "Hackathon" });
    cy.visit("/hackathon.html");
    cy.eyesCheckWindow("Login page");
    cy.eyesClose();
  });
});
```

From 18 lines of code to just 8! I didn't had to inspect any DOM elements. All I needed to do was visit the login page and then take a screenshot of it. In addition, when I ran the same test on the app's 2nd version, no changes were needed on the code. Applitools took care of this and had flagged this test as "Unresolved" since there are visual differences between the app's 1st and 2nd version. The only thing that I needed to do was to review the screenshot and verify if the changes are valid or not. From the Applitools dashboard, I added bug annotations or remarks for any issues found. Looking very efficient already!

What was also enjoyable about this hackathon was it showcased some of Applitools' advanced AI comparison algorithms to visually compare dynamic data as opposed to existing visual testing libraries which uses pixel by pixel comparison. At my workplace, we have introduced visual testing as an important part of our test strategy and as good as it is finding visual bugs, visual testing has also become one of our testing problems. We have experimented with various visual testing tools in the past but we all ended up with the same problem - test flakiness introduced by pixel by pixel comparison. With Applitools, you can  use different match levels based on your requirements so you have full flexibility on how you want to do visual comparison. 

Overall, this hackathon was an excellent initiative from Applitools to spread the word about automated visual testing. So if you're up for some coding challenge or currently bored or you want to win some cool prizes... then there's still time left to do the hackathon so I recommend doing it! On the other hand, if you wish to learn more about Applitools itself, then I would recommend doing the [Modern Functional Testing](https://testautomationu.applitools.com/modern-functional-testing/) course from Test Automation University. 💜