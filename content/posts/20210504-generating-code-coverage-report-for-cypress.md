---
author: "Marie Cruz"
title: "Generating Code Coverage Report for Cypress"
date: "2021-05-04"
tags:
- testing
- tools
- test-automation
- cypress
---

One of my technical tasks last week at Zoopla was to integrate code coverage for Cypress so we can see which parts of the code are overly tested and which parts are not tested. We're currently trying to refactor the Cypress tests that we have and this will help us understand which tests we can delete and tests that perhaps we need to add. While code coverage is not the best measure for checking that we have written a great test nor it also doesn't measure that our tests are credible, it does give a visual indicator to see which areas of our code has been overly tested and which areas have not been tested at all.

The first thing that I did was look at [Cypress' code coverage documentation](https://docs.cypress.io/guides/tooling/code-coverage#Introduction) which was very informative. If you are not familiar with how code coverage works in a nutshell, I shall explain it briefly.

## How does Code Coverage works?

Code coverage tools work by scanning your application's source code and inserting additional counters to different branches, statements, lines and functions. Every time your automated tests hit different areas on the code base, the counters will increment. Once the tests finished, the total number of hits will then be recorded against the total number of possible hits. 

## Installing Required Dependencies

The first thing that we need to do is install the dependencies that we need to get going. On this post, I'll be using `babel-plugin-istanbul` and `cypress/code-coverage` plugins.

Let's install that by adding the following packages to our project.

```
## using npm
npm i -D babel-plugin-istanbul @cypress/code-coverage

## using yarn 
yarn add -D babel-plugin-istanbul @cypress/code-coverage
```

## Instrumenting Your Code

As per Cypress documentation, there are two ways of instrumenting code coverage.

1. Via nyc's instrumented feature which is done by running a command on your terminal which will then produce instrumented source files.
1. Via babel-plugin-istanbul which is done during your code transpilation if your project is already using babel.

Since our project is using babel already, I opted for using babel-plugin-istanbul and added the following plugin to our .babelrc file to instrument our code base. However, Jest already uses istanbul so using babel-plugin-instanbul will result in a duplicate plugin error. To fix this, I used the `env` options from babel to allow nested configuration. If the environment name matches the key provided, the babel-plugin-istanbul will be used. In this example, I created a new environment key called `e2e`.

```
{
  "presets": ["next/babel"],
  "env": {
    "development": {
      "plugins": ["@emotion"]
    },
    "e2e": {
      "plugins": ["@emotion", "istanbul"]
    }
  }
}
```

## Setting up Cypress Code Coverage

This is actually quite straightforward to do. First, add the following import statement to your `cypress/support/index.js` file

```
// all other imports added here

import '@cypress/code-coverage/support';
```

Next, update your `cypress/plugins/index.js` file to register the code coverage plugin into your Cypress tests.

```
module.exports = (on, config) => {
  require('@cypress/code-coverage/task)(on, config);

  // additional configuration such as webpack options and registered tasks

  // very important to return according to the docs!
  return config;
}
```

In order to use the `babel-plugin-istanbul` from our `babelrc` file, let's set the `BABEL_ENV` to `e2e` on our `cypress.json` file.

```
"env": {
  "BABEL_ENV": "e2e"
}
```

Now, when I ran our Cypress tests, I see additional commands (before and after hooks) on the test runner that code coverage is being collected and a report is generated automatically on `coverage/lcov-report/index.html`.

## Combining Jest and Cypress Code Coverage Reports

The last step that I needed to do was to combine the code coverage reports from our unit tests with the report that Cypress generated.

To do this, I had to make some minor adjustments on our Jest setup and add the following changes. This will save the jest's code coverage report on a folder called jest-coverage for better transparency.

```
module.exports = {
  collectCoverage: true,
  coverageDirectory: 'jest-coverage',
  coverageReporter: ['json', 'lcov', 'text-summary', 'clover']
}
```

I also updated the location where the Cypress code coverage report will be saved by adding the following line to our `package.json` file.

```
"nyc": {
  "report-dir": "cypress-coverage"
}
```

Remember to add these new folder locations to your `.gitignore` file so you don't commit these to your repo! Then, after following Gleb Bahmutov's instruction on his [cypress-and-jest](https://github.com/bahmutov/cypress-and-jest#merge-coverage-reports) github repo, I added the following scripts on our package.json file:

```
"scripts": {
  "copy:reports": "mkdir coverage .nyc_output || true && cp cypress-coverage/coverage-final.json coverage/from-cypress.json && cp jest-coverage/coverage-final.json coverage/from-jest.json",
    "combine:reports": "npx nyc merge coverage && mv coverage.json .nyc_output/out.json",
    "code:coverage:report": "npx nyc report --reporter lcov --reporter text-summary --report-dir coverage", 
    "generate:code:coverage:report": "yarn copy:reports && yarn combine:reports && yarn code:coverage:report"
}
```

Let's look at the new commands in detail.

- `copy:reports` - this creates `coverage` and `.nyc_output` folders and copies the respective json files from `cypress-coverage` and j`est-coverage` to the `coverage` folder.
- `combine:reports` - using nyc's merge command, we are merging the two json files and then moving the merged json file to `.nyc_output`.
- `code:coverage:report` - using nyc's report command, we are simply generating the final html report.
- `generate:code:coverage:report` - this is just a handy command for running the above three commands in one go.

Here is what the final report looks like.

<figure>
  <img src="../../images/report.png" alt="An HTML report showing code coverage from both Jest and Cypress.">
  <figcaption>Code Coverage Report from Jest and Cypress</figcaption>
</figure>

## Wrapping Up

The Cypress code coverage plugin is a great plugin for saving the code coverage during your test runs. However, it doesn't instrument your code so you have to do this separately. The good news is if your team is using babel already, then the `babel-plugin-istanbul` works like a charm. 