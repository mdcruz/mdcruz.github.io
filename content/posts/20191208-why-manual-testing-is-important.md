---
author: "Marie Cruz"
title: "Why Manual Testing is important"
date: "2019-12-08"
tags:
- testing
---

When I started my career as a Graduate Test Consultant, I started off learning about manual testing and all the different types of testing techniques such as equivalence partitioning and boundary value testing. While I initially enjoyed the joys of devising a manual test script and testing the application by following the steps that I have written one-by-one, overtime I found this very time consuming and not challenging since the steps are very repetitive. After a year, I decided to switch to test automation instead because I wanted to do a more challenging role and during this time, there was more demand to hire automation engineers so I wanted to skill up. Plus, I also wanted to avoid manual testing because it was boring 😅. 

Fast forward to present time, I am still doing manual testing! The difference now is that I have learned to embrace it. While it's great that test automation has given me the skills that I needed to automate different tests and catch regression bugs that I would have missed if I was manually testing a feature, manual testing allows me to find bugs that are not caught by automated testing. 

In all honesty, manual testing is very tedious and boring but only if you are doing it on areas where it should be replaced by test automation instead. I'm talking about regression testing where you check the application's existing functionality to verify that it still functions as expected by executing the same test steps manually again and again every time a new change is deployed. Anything that is repetitive should be automated and this is emphasised more as companies embrace DevOps ways of working. While some may agree that using just test automation on its own is adequate enough to provide a high level of confidence in shipping a product to production, I learned through my personal experiences that testers need to use both manual and test automation to effectively deliver a quality product. Moreover, that test automation does not replace manual testing but complements it instead.

A key strength of manual testing is the ability to find bugs that were missed during requirements gathering stage. For example, [exploratory testing](https://www.testingexcellence.com/exploratory-testing-important-agile-projects/) is a technique which complements test automation because it allows tester to discover and learn the application that they are testing via ad hoc tests and checking the application without following a structured test case. Exploratory testing forces you to think like real users as different users will have different ways of using your application while automated tests accepts a set of predefined inputs (defined by yourself and not necessarily by a real user). Essentially, exploratory testing encourages your curiosity and creativity as you are not constraint by any rules or requirements. At the same time, exploratory testing is only effective if you, as a tester, are willing to use your intuition to make reasonable judgment.

As an example, the team that I am part of had implemented a list component. To showcase how the component works, we have a playground feature in our website which allows a user to customise their list. As part of our automated regression testing, a test had been added by one of our developers to verify that users can add a new data on the list by clicking the “add” button and that they are also able to delete it by clicking the “remove” button. Looking at the below screenshot, we can see that the flow works as expected. We are also confident that if the automated test fail in future builds, then we would have caught the issue at an early stage.

<figure>
  <img src="../../images/newskit.png" alt="NewsKit UI adding data to a list">
  <figcaption>Adding and deleting data from list works as expected</figcaption>
</figure>

However, different users will have different ways of interacting with your application. For example, user A will use the “remove” button to delete their data from the list but user B might not choose to click the “remove” button. Instead, user B might do this by deleting the characters from the input field. By looking at the screenshot below, there seems to be a bug after user B has deleted his data. When the “add” button is clicked to try to add a new data, no input field is added but the list is still incrementing. This is where exploratory testing flourishes as this bug would not have been caught by your automated test. 

<figure>
  <img src="../../images/newskit2.png" alt="NewsKit UI adding data to a list with blank items">
  <figcaption>List has incremented to 3 but data is blank</figcaption>
</figure>

While it's great that test automation has given as the ability to continuously test our application and provide a fast feedback loop to our team, manually testing your application is equally just as important. Exploratory testing is a great way to uncover hidden bugs and increases your confidence that when you ship your application to production, there will be less bugs to worry about (because we know that there is no such thing as a bug free application! 😬).

For further reading regarding exploratory testing, check out the links below:

- https://governmenttechnology.blog.gov.uk/2014/11/28/top-10-tips-to-exploratory-testing/
- https://www.softwaretestinghelp.com/what-is-exploratory-testing/
