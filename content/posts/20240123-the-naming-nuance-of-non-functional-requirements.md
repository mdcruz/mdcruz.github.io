---
author: "Marie Cruz"
title: "The Naming Nuance of Non-Functional Requirements"
date: "2024-01-23"
tags:
- testing
- dzone
- thought-leadership
---

This post was originally published on the [DZone](https://dzone.com/) site which you can view [here](https://dzone.com/articles/the-naming-nuance-of-non-functional-requirements).

In software testing, we have traditionally grouped requirements into two main areas: functional and non-functional. Commonly, people define [functional requirements](https://dzone.com/articles/functional-vs-non-functional-requirements-the-full) as to how the system should work. In contrast, non-functional requirements refer to how well the system should behave or perform. 

But let’s discuss the big elephant in the room. **The name “non-functional requirements” is terrible.** The name has been a source of confusion and may not accurately convey the essence of what these requirements encompass.

When working on software projects, teams put less emphasis on non-functional requirements, which is a grave mistake. If you quickly browse articles online related to non-functional requirements, some authors even mention that non-functional requirements are optional. Non-functional requirements become “non-important requirements,” but I can argue that they are just as important, if not more important, than functional requirements since they can impact the overall quality of your application. 

Neglecting the prioritization of non-functional requirements has tangible repercussions, especially on the user experience. This is exemplified in real-world scenarios such as:

- [Amazon](https://www.cnbc.com/2018/07/19/amazon-internal-documents-what-caused-prime-day-crash-company-scramble.html#:~:text=Amazon%20failed%20to%20secure%20enough,Amazon%20documents%20obtained%20by%20CNBC.) encountered system issues during Prime Day in 2018, struggling to handle a substantial surge in traffic, resulting in a compromised user experience.
- [Nationwide](https://www.computerweekly.com/news/252513995/Nationwide-suffers-payday-payments-outage), a major bank in the UK, experienced a payday payment outage in 2022. This unfortunate incident left thousands of customers unable to access their accounts and fulfill essential financial transactions, underscoring the critical importance of robust non-functional considerations.
- [Pokemon Go](https://www.reddit.com/r/TheSilphRoad/comments/15tqedq/pokemon_go_has_become_very_unstable_and_crashes/#:~:text=It%20will%20crash%20on%20startup,but%20it%20does%20for%20me.), a popular game released in 2016, still gets reports of their application crashing from gamers.

How can we ensure that [non-functional requirements](https://dzone.com/articles/non-functional-requirements-how-to-improve-perform) deserve the spotlight they deserve? In this article, I’ll talk about the naming nuance of non-functional requirements and explore if there is a better terminology for the dreaded term “non-functional.” Focusing on this terminology shift is not merely about semantics but aims to reflect better the comprehensive nature of these requirements and their impact across various system dimensions.

## Naming Nuance

It is no secret that the tech industry has had its share of horrible naming conventions, one example being the slave and master terminologies used widely for decades in technical contexts. The naming for non-functional requirements is also another example. 

If you look up the term non-functional in a dictionary, the Cambridge Dictionary defines non-functional as “not working, or not working in the correct or usual way.” 

If non-functional refers to something that is not working, why did we adopt non-functional requirements as an industry-wide term in the first place? Most users will deem the functional requirements unusable without the so-called non-functional requirements. Suppose your application has a login feature that works well from a functionality perspective but is not keyboard-accessible or responsive when the incoming traffic is higher than usual. In that case, the login functionality will be unusable to most users.

We work on projects where performance, scalability, availability, accessibility, and security, qualities that fall under the umbrella of non-functional requirements, are essential. However, this slight naming nuance is more harmful than good, especially when educating people about this topic, since it leads people to believe these qualities are less important.

<figure>
  <img src="../../images/functional-vs-non-functional.png" alt="On the left is a list of checked tasks representing functional requirements. On the right is a list with crossed tasks to represent non-functional requirements.">
  <figcaption> Functional requirements are important, and non-functional requirements are considered less important or optional. </figcaption>
</figure>

Non-functional requirements remain a widely accepted and recognized term in the industry, and teams understand the importance of neglecting these requirements. But, as an industry, we should frame non-functional requirements better, even though we do not intend to treat non-functional requirements as less important.

## Naming Alternatives

Non-functional requirements often encompass the whole system rather than specific behavior. For example, backend performance refers to how well your backend application can process user requests under a given workload. Scalability attribute refers to how well your system can expand its capacity to handle growing demands. In contrast, the resiliency attribute refers to how well your system reacts to failures. These non-functional requirements, just like any other non-functional requirements, look at a system as a whole.

### Cross-Functional Requirements

A better naming alternative is to call them “cross-functional” requirements, which [Sarah Taraporewalla](https://www.linkedin.com/in/sarahtaraporewalla/) coined in her blog post [I don’t believe in NFRs](https://sarahtaraporewalla.com/agile/design/i-dont-believe-in-nfrs), which has been adopted widely in Thoughtworks. By adopting the term cross-functional requirements, there is a deliberate shift in perspective. 

Cross-functional emphasizes that these requirements are not secondary or less important but are integral to the entire functioning of the system. The cross-functional name implies that these requirements cut across various functions and aspects of the system, ensuring it performs well as a cohesive unit. In addition, changing the name to cross-functional communicates a holistic approach to system development, encouraging teams to prioritize and integrate these considerations from the outset, ultimately leading to a more robust and reliable end product.

### Human Qualities

Apart from cross-functional, [Jeff Nyman](https://www.linkedin.com/in/jeffnyman/) proposed to call non-functional “human qualities” instead when he crafted this insightful article on [“reframing non-functional as human qualities”](https://testerstories.com/2018/06/reframe-non-functional-as-human-qualities/) to represent the different human feelings that we feel when using the system and how it would negatively make us feel when so-called non-functional requirements are not prioritized. 

Similar to how a person's qualities collectively shape their character, non-functional requirements also shape the character of a system as a whole. Naming non-functional as human qualities emphasizes their impact on the overall user experience and system quality from a user's perspective. In addition, the term human qualities can make these technical aspects more relatable and understandable to a broader audience, including non-technical stakeholders.

### Quality Aspects

[Beren Van Daele](https://www.linkedin.com/in/berenvd) refers to all requirements as “quality aspects” as part of the RiskStorming workshop to force testers to think about all the aspects that can impact the quality of your application. Each non-functional requirement represents a specific aspect that, when fulfilled, enhances the system's overall quality. Here is an illustration created by software tester [Jędrzej Kubala](https://www.linkedin.com/in/jedrzej-kubala/).

<figure>
  <img src="../../images/quality-aspects.png" alt="A diagram showing the different quality aspects important to your projects.">
  <figcaption> Quality aspects represent the requirements important to your projects, including the common attributes grouped as part of non-functional requirements. </figcaption>
</figure>

## Beyond the Name

Personally, I prefer the term cross-functional requirements since it’s more apparent that there are requirements that cross multiple areas of your application. Whichever naming alternative you use, teams should still make a lot of effort to ensure that these requirements are prioritized and treated the same way we discuss functional requirements. For example, from a performance perspective, one way to do this is to have a [continuous testing approach](https://dzone.com/articles/a-continuous-testing-approach-to-performance), where various performance activities are embedded throughout the software development lifecycle.

Framing non-functional requirements as cross-functional requirements, human qualities, or quality aspects is a great way to remove the stigma that non-functional requirements are unimportant. 

It may be a name, but this can frame our mindsets differently when we use the correct term.