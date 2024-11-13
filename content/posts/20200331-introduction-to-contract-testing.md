---
author: "Marie Cruz"
title: "Introduction to Contract Testing"
date: "2020-03-31"
tags:
- testing
- contract-testing
---

Microservices architecture has been a buzzword for so many years with a lot of companies adapting to this approach. With Microservices, large software projects are broken down into smaller modules or components, which are developed independently by different teams. Each of the different modules have specific responsibility and has its own infrastructure so changes can be tested and deployed independently. There are numerous benefits over monolithic architecture which has been covered by this [article](https://dzone.com/articles/benefits-amp-examples-of-microservices-architectur) pretty well. With this new approach however, comes new challenges to testing as well. In this blog post, we'll look at why traditional testing techniques might not be the best fit for testing microservices and how we can test microservices efficiently through the use of contract testing.

## Issues with traditional testing techniques

Deploying a lot of small, independent services have benefits but on the other hand, integration testing becomes complex as the integration points between different services increase. Traditional testing techniques like end-to-end integration may seem like the best approach but these tests becomes problematic with testing microservices because these tests are slow, error prone and hard to maintain which leads to a slow feedback loop. To add to this, it requires proper end-to-end environments where services are integrated with each other with the required test data, configuration etc. I guess if you only have two services, this type of testing is fine but imagine if there's about hundreds or thousands of different services communicating to each other like what Amazon or Netflix have? How are you going to ensure that every change you make does not cause issues on other services? How are you going to maintain test data efficiently? 

End-to-end integration tests requires proper test environments to be integrated with each other but implementing this strategy for microservices may end up causing you more issues than values. On the other hand, if we have isolated tests and create mocks for external dependencies, sure the tests will be fast and easy to maintain but how confident are you that the mock dependency is a true reflection of the real service? How can we solve this? The good thing is there is another layer of testing that we can do to help us test microservices efficiently.

## What is Contract Testing?

Contract Testing is a form of testing where you test the contract between the services communicating to each other. 

With contract testing, we have the following terminologies:

* **Consumer** - client or another service that consumes data from a provider
* **Provider** - service that provides data to a consumer
* **Contract/Pact** - a document which serves as the contract between consumer and provider. This is normally in a JSON format and captures what requests the consumer needs from the provider and what data types, status codes, responses the provider will return.
* **Pact Broker** - a hosted service which stores all the contract. This serves as the communication channel between consumers and providers.

The good thing about contract testing is there is no need for integrated environments as the consumer expectations are captured in this contract and validated against a mock provider service. The contract is then uploaded to the Pact Broker and in turn, the provider runs their test and verifies that whatever is written on the contract is correct. 

I like this analogy from [Pact](https://docs.pact.io/), which is a tool that is use for Contract Testing: 

"Do you set your house on fire to test your smoke alarm? No, you test the contract it holds with your ears by using the testing button."

With contract testing, even though we are using a mock service to validate our tests, this still solves the problems of having broken changes deployed to production as consumers are notified when there is a change deployed from the provider which causes the contract to break and vice versa. 

Similar with real life contracts or agreements, if there's a change needed to be done on the contract, both parties will be notified and changes will only be made once everyone is on the same page and approves.

A popular variation of contract testing is consumer driven contract testing. In this scenario, the consumer drives what needs to be on the contract, which I think leads to a better API design. This will reduce the code in the consumer side as it will only test what it needs from the provider. With this approach, the provider is also free to introduce other changes as long as it's not breaking the contract with its consumers. 

## Limitations of Contract Testing

With everything, you can't use contract testing solely when testing microservices and it's not a replacement for actual communications between different teams. Contract testing is not a replacement for functional tests as we are not testing the behaviour of our services. It's also not advisable to use contract testing if you have a public API because you don't know how many consumers you have and how they use your service. 

On the next blog post, we'll look closely how we can write a contract test by using Pact.

