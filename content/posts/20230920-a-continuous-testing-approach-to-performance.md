---
author: "Marie Cruz"
title: "A Continuous Testing Approach to Performance"
date: "2023-09-20"
tags:
- testing
- performance-testing
- dzone
- thought-leadership
---

This post was originally published on the [DZone](https://dzone.com/) site which you can view [here](https://dzone.com/articles/a-continuous-testing-approach-to-performance).

A term you have probably heard a lot nowadays is continuous testing. Continuous testing, explained simply, is about testing everywhere across the [software development lifecycle](https://dzone.com/articles/sdlc-software-development-life-cycle-benefits-stag) and should include activities beyond automation, such as [exploratory testing](https://dzone.com/articles/exploratory-testing-tutorial-a-comprehensive-guide). Continuous testing implies that testing is not shifted but is found at every stage of the software development lifecycle.

This is supported by the famous [Continuous Testing in DevOps model](https://danashby.co.uk/2016/10/19/continuous-testing-in-devops/) created by Dan Ashby, which you can see in Figure 1.

<figure>
  <img src="../../images/continuous-testing-original.jpg" alt="Continuous testing in Devops model by Dan Ashby showing that testing is found at every stage of the software development lifecycle.">
  <figcaption> Continuous testing in the DevOps model. </figcaption>
</figure>

In addition, continuous testing is not to be mistaken for shift-left testing, as these are two different approaches. While the shift-left approach to testing requires you to shift the testing process earlier in the development lifecycle so issues can be found earlier, continuous testing highlights that testing happens at all stages of the project lifecycle and is embedded throughout the infinite software development cycle. Therefore, the shift left approach shouldn't be used as an excuse not to perform continuous testing. Continuous testing goes beyond shift-left testing. 

In this article, I will focus on:

- Challenging how others might define continuous testing.
- How to implement continuous performance testing.

## What Is Continuous Testing?

“Continuous testing refers to the execution of automated tests that are carried out at regular intervals every time code changes are made. These tests are conducted as a part of the software delivery pipeline to drive faster feedback on recent changes pushed to the code repository.”

The above is taken from [BrowserStack's definition of continuous testing](https://www.browserstack.com/guide/continuous-testing-in-devops), which is generally what most organizations also use.

Automated tests executed as part of a build pipeline encompass unit, integration, end-to-end tests, performance tests, and more. While [automation plays a significant role in enabling continuous testing](https://k6.io/docs/testing-guides/automated-performance-testing/), continuous testing is not just about having automated tests run as part of a build pipeline. 

The left side of the continuous testing model in Figure 1 shows that testing happens when you test the plan by getting involved in discussions earlier and challenging requirements. It happens when you test the branch by pulling the code locally and exploring the features or even when testing your team’s branching strategies. It happens when you test the code via code reviews and ensure that automated tests are in place. It even happens during the merging and build process by checking if your build pipelines have built the correct versions.

On the other hand, the right side of the continuous testing model in Figure 1 shows that once you have released and deployed your changes, testing also happens continuously. If you use deployment techniques such as canary releases or blue/green deployments, these are also tested to ensure the deployment is successful. When it’s been deployed to production, real users test the changes continuously. But it doesn’t stop there. Testing also occurs during the observability and monitoring stage. You can gather the metrics and use this data to drive improvements continuously. 

Continuous testing goes beyond automated checks. It could also be found beyond the continuous testing model, such as testing ideas before the software development lifecycle. Continuous testing needs to be built in team cultures with openness to learning, collaboration, and even experimentation. Team members must be encouraged to try different approaches and experiment on which works best for their testing needs. 

## How To Implement Continuous Performance Testing

Before I set some guidelines on implementing continuous performance testing, let’s look at how performance testing is done traditionally and explore why this doesn’t scale well nowadays.

Traditionally, performance testing is seen as an activity you do right at the end before releasing it to production. It’s done after you have verified the main functionalities of a system and often requires a specialized group of performance testers, which creates a siloed approach. Since it is left right at the end, any issues found during performance testing are expensive. Trying to fit the traditional performance testing approach into Agile ways of working simply doesn’t work due to the rapid development of features and the need to release these features quickly.

So, how can you implement a continuous approach to performance testing? Is it by introducing automated performance tests that are triggered automatically when there are new changes added? While this is important, remember that continuous testing is more than just automation. 

The following sections give an overview of how you can incorporate performance testing across all stages of the software development life cycle, with reference to Dan Ashby’s continuous testing model in Figure 1. The sections below are not set in stone but just guidelines, so always consider the context of your projects.

### Plan

Performance testing requirements need to be discussed as early as possible and incorporated as part of a user story to increase awareness. From personal experience, if your team has a performance champion, it is easier to convince the rest of the team why performance testing needs to be done earlier. A performance champion can be part of a wider Community of Practice, especially focused on performance-recommended practices that educate different teams.

<figure>
  <img src="../../images/performance-activities-plan.png" alt="Performance testing activities during the planning stage.">
  <figcaption> Performance testing activities during the planning stage. </figcaption>
</figure>

Make it a practice to discuss performance requirements as part of every feature and create acceptance criteria for it based on existing Service Level Agreements (SLA) and Service Level Objectives (SLO) that are in place. If there are no SLAs or SLOs, collaborate on what these should be.

Consider including performance requirements in your definition of done (DoD). This will increase awareness that performance is included continuously in every feature that is being developed. If your teams are practicing [three amigos](https://www.agilealliance.org/glossary/three-amigos/) or [story-shaping sessions](https://www.youtube.com/watch?v=XQn0WdHbVtg) to expand requirements, ensure that everyone understands what is expected from a performance point of view.

### Branch and Code

You and your team can also investigate the code and check for any possible performance bottlenecks that might arise. This is also where it’s useful to write automated performance tests at the same time as the code is being developed. Having a modern performance testing tool can help as you get wider buy-ins if writing performance checks offer a great developer experience. 

<figure>
  <img src="../../images/performance-activities-code.png" alt="Performance testing activities during the branching and coding stage.">
  <figcaption> Performance testing activities during the branching and coding stage. </figcaption>
</figure>

Performance checks at this stage can be lower-level component testing rather than a full-blown end-to-end performance. Examples of component-level testing that you can do from a performance perspective include: 

- Focusing on protocol-level tests without involving a UI.
- Targeting specific API endpoints and observing the response times when introduced with a gradual increase in load.
- Finding the breaking point of an API endpoint by performing stress or spike testing earlier on.

Beyond automation, you can also try pair testing and explore the application locally and aim to find performance issues that can’t be caught by automated tests, such as checking the [perceived performance](https://www.testingwithmarie.com/posts/20230206-the-psychology-of-waiting/) of your application. 

### Merge

When developers push their code into the build pipelines and with the introduction of feature environments in some cases, it’s important to include fast and reliable automated tests to enable a fast feedback loop. 

From a performance perspective, you can run the component-level performance tests identified in the previous stage as part of your CI/CD pipelines. You may also want to add the tests you have created as part of a [smoke performance tests](https://grafana.com/blog/2024/01/30/smoke-testing/) suite that verifies that your application can operate when exposed to a minimal load. From a backend performance perspective, these tests should run with fewer virtual users or shorter duration. If you are focused on front-end or client-side performance, you can also run basic checks on a few pages to get data using tools specifically for client-side performance. 

<figure>
  <img src="../../images/performance-activities-merge.png" alt="Performance testing activities during the merging stage.">
  <figcaption> Performance testing activities during the merging stage. </figcaption>
</figure>

To have better confidence, you can use [performance heuristics during exploratory testing](https://www.youtube.com/watch?v=6wVwdlMwsJ8) once changes are deployed to an environment (this could also be done earlier!).

### Build

When features are merged into your main branch, this is where you can run more performance checks that are closer to what your user experiences. Instead of component-level performance tests, you can focus on end-to-end performance tests because you still need to verify the main user flows that your typical user would perform. Examples of end-to-end testing that you can do from a performance perspective include: 

- Running your [average load tests](https://grafana.com/blog/2024/01/30/average-load-testing/) automatically to assess how the system performs under a typical load whenever the code is deployed to a staging environment. 
- Running end-to-end performance tests to simulate a user journey flow and find blind spots on the browser level whenever a backend service is exposed to a high load. At this stage, your performance tests are more realistic and try to simulate what a typical user would do.
- Optional stress, spike, or soak tests that can be triggered manually on your CI pipeline 
- If you have integrated your results into a visualization dashboard, you can continuously observe the performance trends and use the data to inform your team whether it can be deployed to production safely.

<figure>
  <img src="../../images/performance-activities-build.png" alt="Performance testing activities during the building stage.">
  <figcaption> Performance testing activities during the building stage. </figcaption>
</figure>

### Release and Deploy

When features are released in production, you can perform health checks to verify if the deployment has been successful. 

<figure>
  <img src="../../images/performance-activities-deploy.png" alt="Performance testing activities during the release and deployment stage.">
  <figcaption> Performance testing activities during the release and deployment stage. </figcaption>
</figure>

Some companies also don’t have pre-production environments suitable for performance testing. In this case, you can test in production but do it safely and in a limited capacity because you don’t want it to be disruptive to your users. For example, you can perform load tests during an agreed window, normally during off-peak hours.

### Operate and Monitor

Once your code is live and in production, your users continuously test it. To understand user pain points related to performance, have a channel where you can get their feedback and incorporate it into the next iteration of your software development life cycle. This also allows you to learn more about your system based on how different people use it, which leads to testing ideas you can apply in the next cycle.

<figure>
  <img src="../../images/performance-activities-monitor.png" alt="Performance testing activities during the operation and monitor stage.">
  <figcaption> Performance testing activities during the operation and monitor stage. </figcaption>
</figure>

Having a monitoring solution is also one way of continuously testing your application. This gives you real-time results on any performance errors that your users might experience, whether it’s any pages that have slower response times or any failed requests that might be happening on different data services.

## Conclusion

Continuous testing allows us to learn about our system in all stages of the software development lifecycle. From a performance perspective, continuous testing doesn’t mean more automated performance tests. Rather, it’s about embracing that in each stage of the software development life cycle, performance needs to be embedded and improved continuously.