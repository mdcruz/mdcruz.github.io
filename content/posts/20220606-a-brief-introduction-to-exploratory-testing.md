---
author: "Marie Cruz"
title: "A Brief Introduction to Exploratory Testing"
date: "2022-06-06"
tags:
- testing
- thoughts
---

A few months back, I had to help out the mobile app team in Zoopla and introduced a Quality and Testing strategy document that the team can refer back to. One of the testing types that I introduced them to was Exploratory testing and I explained the benefits of it, how to use heuristics and shared with them how I structure my testing notes. While my blog posts have been predominantly about test automation, I'm dedicating this post about exploratory testing and why every software tester should do it.

## What is Exploratory Testing?

Exploratory Testing is simultaneously learning about the system while designing and executing tests, using feedback from the last test to inform the next. When doing Exploratory testing, you test and execute it at the same time while simultaneously learning about the behaviour of our application. Exploratory testing is not playing around with the system in an unplanned or unstructured way.

## Why do we need to do Exploratory Testing?

- To learn more about the application under test. Exploratory testing improves our investigation skills.
- To adjust testing for risks. With scripted testing, the test remains the same even though the risks have changed.
- To increase shared understanding among the team
- To uncover quality information about the application
- Encourages collaboration amongst the team
- Scripted testing (test case) and test automation are just one side of the testing approach. Exploratory testing complements this approach.

## Is it the same as Adhoc Testing?

There is a general misconception that adhoc testing or unplanned testing is the same as exploratory testing. 

The goal of adhoc testing is to find as many bugs as possible. You click around the application and try to find issues on different areas as much as possible. There is no plan of action but to just use the application and try to break it.

The goal of exploratory testing is to better understand the application. For example, if you don’t know much about the feature being developed and you want to know how it behaves, you can explore the feature using different inputs. Exploratory testing is also not limited to just testing the application, you can also do exploratory testing even before developing the application by testing out ideas, acceptance criteria, designs or artifacts.

This excellent blog post, [Why adhoc testing is not exploratory testing](https://callumakehurstryansblog.wordpress.com/2021/11/02/why-adhoc-testing-is-not-exploratory-testing/), by Callum Akehurst-Ryan describes what the main differences are between adhoc and exploratory testing.

## General tips for better Exploratory Testing

- Use a charter template such as this [Charter Template](https://gist.github.com/MelTheTester/aa112ba37e1ccc0672f760db271fb8aa#file-charter-template) to guide your exploratory testing.
- Share your notes actively to the team. Your notes can be of different formats (e.g. mind maps, Google doc, spreadsheet, confluence page, etc) but the main thing is, sharing testing notes can help you generate new test ideas and techniques. Your testing notes can also be used by your team if they need to do some testing from their side. 
- Make sure to only focus on a specific goal. One way to do this is to have a charter definition which follows this format of "Explore <Target> with <Resources> to discover/learn <information>".
Time-box your exploratory testing session and ideally keep it just under 90 mins and don’t deter from your charter. 
- When creating your testing notes, share screenshots clearly with annotations if needed or even better, share videos.
- Use a heuristics cheat sheet to help structure your exploratory testing. More information on the next section about heuristics.
- Categorise your testing notes well and it’s up to you to decide how you do this! You can use various emojis (e.g. 🎉 for praise, ❓ for questions, 🐞 for bugs).

## Bonus: What are Heuristics?

Heuristics are cognitive shortcuts that we can use to help with Exploratory testing. Heuristics are useful when you are in a situation where you need to test an application that you don’t have any context and there is little to no documentation that can help you. Heuristics can guide you on what test you should do, which area of functionality you need to focus, or what data to use. 

Some examples of popular heuristics include:

- Goldilocks - “Too big, too small, just right”. This works well with testing input fields (too many characters, too less, just the right amount).
- [SLIDE](https://blogs.qub.ac.uk/digitallearning/2020/07/06/accessiblepresentations/) (Styles, Links, Images, Designs, Evaluation) for accessibility
- [Mobile App Testing](https://www.ministryoftesting.com/dojo/lessons/mobile-app-testing-mnemonic?s_id=10395137) Mnemonic

## Wrapping Up

Exploratory testing is a skill that every tester should know about. We can't just rely on scripted tests and test automation. If we want to make sure that we are testing for risks continuously, a solid approach to testing should also include Exploratory testing.

## Recommended Resources

- [Explore It!](https://www.amazon.co.uk/Explore-Increase-Confidence-Exploratory-Testing/dp/1937785025) by Elizabeth Hendrickson - The holy grail book for Exploratory Testing!
- [30 Tips to Supercharge your Exploratory Testing Efforts](https://www.ministryoftesting.com/dojo/lessons/explore-galore-30-tips-to-supercharge-your-exploratory-testing-efforts) by Simon Tomes
- [Test Heuristics Cheat Sheet](https://www.ministryoftesting.com/dojo/lessons/test-heuristics-cheat-sheet) from Ministry of Testing
- [Twitter thread](https://twitter.com/CAkehurstRyan/status/1531983911160631297) by Callum Akehurst-Ryan about Exploratory testing resources
