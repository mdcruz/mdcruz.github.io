---
author: "Marie Cruz"
title: "What I learned at Software Testing Industry Forum 2020"
date: "2020-03-03"
tags:
- testing
- thoughts
---

Last Tuesday, 25th February 2020, I attended [Software Testing Industry Forum](https://www.softwaretestingnews.co.uk/industryforum/) for the first time. It was a day full of great talks and networking with old acquaintances and making new ones. It was great to hear experiences from other QA professionals and learned about new products and services that companies can use to improve the quality of their products.

After dropping the little one off at nursery, I headed straight to British Museum to register and already saw a couple of familiar faces. After networking for some time, it was time to attend the talks.

## Quality Assurance in the Age of DevOps, Fabian Basciani, JP Morgan

The first talk I attended was a talk by Fabian where he talked about Quality Assurance in the Age of DevOps, sharing his experiences on how his team achieved a true DevOps transformation by using the test automation pyramid concepts and combining it with techniques such as event sourcing model. His talk was a great example on how to implement "Shift Left" where they transformed their development team to be technologists instead. The technologists are responsible for their code, even writing automation tests for it and deploying it to production. They apply principles such as having no dependency on specific test data and minimum dependency on third party systems so their tests can provide a fast feedback loop. There was a good debate at the end as one attendee asked Fabian about his thoughts on using feature flags. To him personally, he doesn't like using feature flags as this increases the number of scenarios they need to test exponentially. My personal opinion is that feature flags are ok to use because it provide companies the benefit to turn a feature on/off on production quickly and safely.

## The Customer is Always Right – Insight-Driven Performance Engineering, Daryl Elfield & Xavier McGlynn, KPMG

This talk was really informative because working on a media publication company with websites that has over 8.5 million active users daily on average, customer centric testing is very important to us for our product to stay relevant. Performance improvement is something that our team has put an important focus on so when Daryl talked about how humans now have less attention span - down to 8 seconds, this made me realised that performance testing is something that we should definitely put a great effort on. 

Daryl and Xavier then talked about how testers should move from using a system centric approach to customer centric approach when testing a feature. With this approach, data is king since this will act as our inputs to our tests. We, as testers, should understand what our customers want and use customer analytics tools frequently. Once we understand what the data is (customer demographics, devices and browsers they use, peak periods, user concurrency to name a few), this can help in shaping our test scenarios, especially in performance testing. KPMG takes this further by developing an in-house NFT (non functional testing) service which companies can benefit from.

<figure>
  <img src="../../images/kpmg.png" alt="KMPG presenting their performance testing approach">
  <figcaption>KPMG's Customer Centric Performance Testing Approach</figcaption>
</figure>

## That’s Not My Job – I’m Not a Tester!, Taheera Atchia, CDL

This was one of my favourite talk from this conference! Taheera's presentation was very relatable and she nailed all the pain points that I have as a QA professional trying to advocate for quality as everyone's responsibility. Testing is everyone's responsibility but we still hear excuses from different people as to why it's not. We should try to unleash the tester within the people from our team and encourage their curiosity. She went on to mentioned that we should be providing guidance (just enough) to get them started and pair pair pair! This made me realised that I need to do more testing sessions within our teams to understand if everyone are aligned with our test strategy.

She also debunked some of the common myths about testing such as developers can't test or pairing is less productive. Trying to change everyone's mindset will be challenging so it's best to start small, build empathy and a shared understanding and of course, celebrate small successes.

## What is your passion – do you bring this to work, Al Lines, Moody's

Al's talk was very refreshing. I wish I was as confident as he was on public speaking! I can see that he is very passionate as a tester and as a person. The main thing that I learned from his presentation was that we should bring the very best in our work and show that we care. For example, to test our features much better, we should always have the best data. When we log bug reports or communicate to the team, we must be detailed so everyone can understand what the issues are. If we don't care, then perhaps we should not be in QA and it's time for a change?

## Contract Testing – The route to Continuous Deployment, Lewis Presscott, Cancer Research

Lewis did a great talk on giving an introduction to [contract testing](https://docs.pact.io/) and how it's different from integration and end-to-end testing. Since we are currently using contract testing in one of our projects, I wanted to hear other people's experiences on how they have used Pact as a contract testing tool. I liked that he involved the audience on his presentation by having an interactive survey, asking us what we think contract testing is and what scenarios it tests. I also liked that he did a little demo of PactFlow and also showed a sample code on how to write a contract test using PactJS. Only thing that I wished he had talked more is how they integrated it on their build pipelines as I think this is something that most testers would also be interested to know more of.

<figure>
  <img src="../../images/lewis-prescott.jpg" alt="Lewis Prescott talking about contract testing">
  <figcaption>Lewis Prescott on Contract Testing</figcaption>
</figure>

## Takeaways

Overall, I learned a lot from all the different talks. It's always nice to hear from other people how they are transforming their quality processes and to also know what tools and services are currently available to accelerate your testing. It was also good to see old colleagues, meet new ones and talk about our personal experiences on what's working well and what's not working well with our current testing processes and ideas on how we can improve. And of course, the most important takeaway, the free food was amazing! 😄 