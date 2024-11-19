---
author: "Marie Cruz"
title: "Top 10 Accessibility Testing Tools for Websites"
date: "2022-08-24"
tags:
- testing
- tools
- accessibility
- applitools-blog
---

This post was originally published on the [Applitools](https://applitools.com/) blog which you can view [here](https://applitools.com/blog/top-10-web-accessibility-testing-tools/).

Learn how to get started with web accessibility testing with this list of the best paid, free and open source accessibility testing tools.

If you visit the [Web Accessibility Evaluation Tools List](https://www.w3.org/WAI/ER/tools/), there are a whopping 167 tools to choose from! As someone who is just starting in the world of accessibility, how do you decide which tools to choose?

There are of course various factors to consider when choosing an accessibility tool, but in this blog post, I’ll share a few tools that I have found really useful when it comes to accessibility testing. If you tuned in to my talk on [Shifting Accessibility Testing to the Left](https://applitools.com/event/shifting-accessibility-testing-to-the-left-2/) (or read my [blog post](https://www.testingwithmarie.com/posts/20210831-shifting-accessibility-testing-to-the-left/)), you would know that I like to group the accessibility testing tools in different areas. These areas are:

- Tools requiring human assistance
- Semi automated tools in the form of browser extensions
- Automated tools

## Accessibility Testing Tools Requiring Human Assistance

Even though there’s a growing number of automated tools out there, accessibility testing still requires human assistance to make sure that the experience we are testing closely matches the one our users will have. The following tools are my go-to when it comes to manually testing for accessibility.

### Keyboards

The first tool is my very own keyboard. Making sure that your website is keyboard friendly and compatible already makes it more accessible than many other websites.

To get started with keyboard compatibility testing, you need to know basic keystrokes such as `TAB`, `Enter`, `Arrow keys (← ↑ → ↓)`, just to name a few, to make sure that you can still interact with the website as if you are using a mouse.

When it comes to testing with a keyboard, this can surface accessibility considerations such as:

- Are your elements focusable when the user tabs through it?
- Do you have a skip to the main content link which is only visible when tabbed through with a keyboard?
- When presented with a modal or a pop-up, can the users exit the modal and interact with what they were presented before?

### Screen Readers

Using a screen reader can be overwhelming for people who don’t use it but screen readers are a must when it comes to testing for accessibility. Depending on the operating system that you are using, there is an available screen reader software for you to use such as [VoiceOver](https://www.apple.com/voiceover/info/guide/_1121.html), [JAWS](https://www.sightandsound.co.uk/product/jaws-home/), [NVDA](https://www.nvaccess.org/) and [TalkBack](https://support.google.com/accessibility/android/answer/6007100?hl=en-GB). Spend some time familiarizing yourself on how to use a screen reader to make sure that your websites are accessible for these users.

### Zoom/Magnification

Users who have low vision need to have a way to easily perceive, navigate and interact with the content that is presented to them. By using the zoom or magnification too that’s built into browsers, you can zoom in up to 200% (or more) and verify if the elements are displayed nicely and are still interactable.

## Semi-Automated Accessibility Testing Tools in the Form of Browser Extensions

Browser extensions are a quick way to help you surface any accessibility issues that your websites might have. Most of the browser extensions are provided to you for free with additional features unlocked if you purchase their commercial version. The common thing with the extensions below is they all provide an easy way to check violations. All of the extensions, apart from ColorBlindly, also provide an easy to digest output so you can share with your teams a list of accessibility issues.

### Axe DevTools and WAVE

[Axe DevTools](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd) and [WAVE](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh) are two extensions that you can install and integrate easily in your browser of choice. These accessibility extensions scan a specific web page and report any accessibility violations that it finds. These are great tools to get started with accessibility, especially if you are a beginner. They provide useful information such as the description of the accessibility violation, its impact, how to fix it and elements that are impacted.

### Google Lighthouse

[Google Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/), which is already built into Google Chrome’s developer tools, provides an easy way to perform accessibility audits. It can also measure web performance apart from accessibility and can provide recommendations on how to fix the issues caught. Don’t get too fixated though with the Lighthouse accessibility scores as this is not a complete indication that your website is accessible.

### ColorBlindly

[ColorBlindly](https://chrome.google.com/webstore/detail/colorblindly/floniaahmccleoclneebhhmnjgdfijgg?hl=en) is an easy to use Chrome extension that can simulate different types of color blindness with just one click. This extension can help you verify if the color schemes of your website are accessible from a wide range of color blindness.

## Automated Accessibility Testing Tools

Now, in order to shift accessibility testing as early as possible, apart from having early conversations with your team, leveraging automation is key so that you can focus on areas where accessibility testing is needed the most. What’s common with these tools is that you can easily integrate them as part of your continuous integration pipelines and it can provide a safety net so your team can be confident in making changes or introducing new features.

### Axe CLI

Command line lovers, this tool is for you! [Axe CLI](https://www.npmjs.com/package/@axe-core/cli) is a command line tool which provides a way to perform accessibility audits straight from your command line. This is particularly useful if you want to quickly scan various pages from your command line.

The scan can be configurable in the sense that you can disable certain accessibility rules, include or exclude certain elements and modify the accessibility report. 

If you’re looking for a quick tool that you can easily integrate as part of your pipelines, give Axe CLI a try.

### Cypress and cypress-axe

Good news for Cypress users! Did you know that you can easily integrate accessibility tests just by installing a plugin called [cypress-axe](https://www.npmjs.com/package/cypress-axe)? Cypress-axe uses the axe-core library and lets you audit pages or components straight from your Cypress tests. I have discussed this in more detail in my course [Test Automation for Accessibility](https://testautomationu.applitools.com/accessibility-testing-tutorial/) so if you’re interested to find out how the plugin works, check out this course from Test Automation University.

Similarly, if you’re using other testing frameworks, you’re also in luck because axe-core can be integrated with other frameworks or testing libraries. Whether you are using Playwright, WebdriverIO, Selenium or others, axe-core has a library for you which can be found here: [projects that use axe-core](https://github.com/dequelabs/axe-core/blob/develop/doc/projects.md).

### Applitools Contrast Advisor

Did you know that Applitools also supports accessibility testing? If you’re already using Applitools for [visual testing](https://applitools.com/blog/visual-testing/), then you can also try their [Contrast Advisor](https://applitools.com/contrast-advisor/) tool which can detect contrast violations using artificial intelligence. With Contrast Advisor, it can easily integrate into your existing workflow and pipelines already so no additional coding setup is needed. You can also validate the contrast of images and native applications easily with this tool.

## Wrap Up

The tools above are by no means a complete list, but should help you get started when it comes to accessibility testing. Regardless of what tools you choose, you should have the same goal and that is to catch as many accessibility issues as possible before giving it to your real users. 

By using a combination of these tools as early as possible, along with other accessibility testing strategies, you can ensure that your user experience is inclusive. The above tools should not be a replacement for accessibility testing with real users but should complement it instead.

## How Can I Help?

Accessibility doesn’t start and end with tools. It requires a change of culture and wider buy-ins to make sure that everyone is on the same page. If you or anyone from your team requires specific consultation help with regards to accessibility, I’m happy to have an introductory chat to help you nurture accessibility within your team. You can contact me via <a href="mailto:marie.descruz@gmail.com"> email</a> or [LinkedIn](https://www.linkedin.com/in/mariedesireecruz/).
