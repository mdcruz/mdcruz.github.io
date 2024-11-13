---
author: "Marie Cruz"
title: "Bug report: Tips, Dos and Don'ts"
date: "2019-11-13"
tags:
- testing
---

As testers, one of our responsibility is to find bugs on the application we are testing to ensure that the quality of the product is what the users expect. As a result, we deal with reporting bugs almost on day-to-day basis. From the outside, writing a bug report/ticket seems to be simple. However, if a bug report doesn’t have much detail, then this just gets piled on the backlog, not prioritised and not fixed which defeats the whole purpose of raising the bug in the first place. It’s also note to mention that not only testers raise bugs. A bug can be raised by anyone from the team and the bug report might end up with different formats based on who raised it.

Last week we had our usual team tech analysis session and we came across a bug ticket reported by an external team that uses our API. The ticket (surprise, surprise!) does not have a lot of great detail added into it. There is no steps added to reproduce the issue, no information which environment this was found or who from the external team reported it. After a couple of seconds, everyone from the team had agreed to put it at the bottom of the backlog.

Imagine if you are task to test a feature but for whatever reason, you are only provided a high level description without any acceptance criteria. You would immediately not prioritise this work because of the lack of information presented to you. The same case applies to developers. Developers are very busy people and if they see a bug report that looks incomplete, then their immediate reaction would be to not spend some time working on the bug fix. 

## Tips on writing a good bug report

So now that we have established that writing a good bug report is important, here are some tips which I find useful that hopefully should help you and your team.

1. **Make your ticket title short and clear but descriptive** – a lot can be understood immediately if your ticket title is clear. A bug title should give a developer or anyone from your team enough context to understand what the bug is. *[Example: ShareBar component on NewsKit site does not render well on small mobile viewport.]*
1. **Add a high level description of how you found this bug to provide more context to the person reading your report**. From reading the description, a developer should be able to understand on a high level what conditions were the bug reproduced. *[Example: The ShareBar component on the NewsKit site is visually cut off when viewed on a small mobile viewport like iPhone 5 which impacts the usability of the component.]*
1. **Add clear information as to which environment and/or device the bug was noticed**. Bugs, especially visual bugs, behave differently on different browsers and devices. Some bugs are also environment specific. For example, it could be related to data setup or access permission on why the bug is happening on Staging environment and not on Dev environment. The more information added on the environment, the faster it will be to isolate the problem.
1. **Add clear step-by-step details on how to reproduce the bugs including any pre-requisites**. Anyone should be able to reproduce the bug easily by following the step-by-step instructions. It should also contain pre-requisite information such as login data, ID use to query an article or the test environment link used.
1. **Add clear expected results as to what the behaviour should be based on what the business requirements are**. *[Example: The ShareBar component when viewed from smaller devices should not be visually cut off.]*
1. **Add the actual result that you observed which prompted you to raise the bug report in the first place.**
1. **Add supporting information such as screenshots so readers can visually see what the bug is.** For visual bugs, you can also add screenshots of what it should look like as opposed to what it currently looks like now. Design markups can also be attached as supporting information to help developers fix the problem.

## Some Do’s and Don’ts

✅ Do consider creating a bug template that people can clone. This enforces everyone on the team to follow the same template. 

✅ Do proof read your bug report for any typo and errors and also to make sure that you have not missed any important information.

❌ Don’t add multiple bugs in one report. It’s better to keep bugs separate for better traceability.

❌ Don’t raise duplicate bugs when possible to not waste your time. It’s better to check the backlog first if the bug has been raised or not. Better yet, ask your team members if they are aware of the bug being raised before.

✅ If unsure, do double check first with your product owner if it’s a bug in the first place or a new requirement.

❌ Don’t say things like “This bug was missed because developer X forgot to consider this.” It’s not nice to play the blaming game (unless you want to be on their hate list..) and this brings down the team morale.

✅ Do reproduce the bug multiple times to ensure that it’s reproducible when a bug report is to be written.

❌ Don’t make the report too long! No one wants to read a lengthy bug report. Keep it short, straight to the point and clear.

✅ Do be ready to help developers answer any of their questions.

## Final Notes

With everything, writing a good bug report takes practice and it’s a skill that testers must be good at. Bugs are known to be troublesome to investigate and fix so the more information you can provide to your developers, the better it will be for everyone. A bug report is your form of communication to the entire team and the more efficient the communication is, the faster it will be to deploy these bug fixes out to production.