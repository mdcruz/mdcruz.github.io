---
author: "Marie Cruz"
title: "Test reporting with Cypress and Mochawesome"
date: "2020-06-08"
tags:
- testing
- tools
- test-automation
- cypress
---

I recently completed the [Applitools Ultrafast Grid Hackathon](https://applitools.com/cross-browser-testing-hackathon-v20-1-apply/) and one of the requirements was to collate the test results and write it to a file so it can be easily reviewed. I faced some limitations with this approach so rather than writing the test results to a text file, I decided to integrate with a third party reporting tool instead. Personally, I haven't used [Mochawesome](https://www.npmjs.com/package/mochawesome) so this was the perfect opportunity to learn about it. Test reporting is one of the important things that you need to think about when you do test automation. A great test report should list all the tests that were run, which tests passed and/or failed, the duration of the tests and should also include some error messages to explain why a test failed. At the same time, a report should be easily shared within your team so everyone is aware what the status is of your test execution. In this tutorial, I'll show you how to export the results of your Cypress tests to an HTML file so you can easily visualise your test runs.

## What is Mochawesome?

Mochawesome is a custom reporter that you can integrate into your Mocha tests. It provides a custom HTML report so you can easily see the test results and filter which of your tests pass or failed. 

There are three libraries that you would need to get started.

- mochawesome - this will convert your tests to JSON output
- mochawesome-report-generator (marge) - this will take the JSON output provided by mochawesome and transform it into a HTML report
- mochawesome-merge - this will merge several mochawesome JSON outputs into one. If you only have 1 test file then you don't need this. But in reality, you will always have more than 1 test file in your framework. 🙂

## Integrating with Cypress

Since Cypress is built on top with Mocha, we can use Mochawesome with Cypress. The first thing that you need to do is install the required dependencies.

`npm i --D mocha mochawesome mochawesome-merge mochawesome-report-generator`

Once the above libraries are installed, we would need to make some modifications to our `cypress.json` file as seen below:

```
"reporter": "../node_modules/mochawesome/src/mochawesome.js",
"reporterOptions": {
  "overwrite": false,
  "html": false,
  "json": true
}
```

Let's break down what's happening here.

We are telling Cypress that our reporter should be mochawesome. We are then setting the options on our reporter such as:

- overwrite - set to false because we don't want to overwrite previous reports
- html - set to false because we don't want the html report to be generated after the test run (more explanation why later!)
- json - set to true so that when our test run is finished, it will generate a json file containing details of our test run.

## Running the tests

When you run your Cypress tests, the command you use should still be similar. Notice though that you will see a log from mochawesome saying that the json output of your tests have been saved. By default, this will save it to a folder called `mochawesome-report`. 

Mochawesome will generate the same number of json files depending on how many test files you have, If you have 2 test files in your project, mochawesome will also generate 2 json files. This is the reason why we set the html property in the `reporterOptions` as `false` because we don't want to generate the html report per test file.

Since the `mochawesome-report` folder will be updated with new json files every time we run our tests, we need to delete this directory prior so we don't see json files from previous test runs. To do this, let's add a new command in our package.json file that will remove this directory.

```
"clean:reports": "rm -rf mochawesome-report/",
"test": "npm run clean:reports && npm run test"
```

Now when we run our tests again, we should see the directory removed beforehand.

## Generating the report

To generate the report, we need to add a couple more commands in our package.json file:

```
"merge:reports": "mochawesome-merge mochawesome-report/*.json > cypress-test-reports.json",
"create:html:report": "marge --reportDir TestReport cypress-test-reports.json",
"generate:html:report": "npm run merge:reports && npm run create:html:report"
```

The `merge:reports` commands will merge all the json files from our `mochawesome-report` directory and store them in a new file called `cypress-tests-report.json`. 

The `create:html:report` command will generate the html report from `cypress-tests-report.json` and save it a new directory called `TestReport`.

The `generate:html:report` command is just my preference and is completely optional. It simply combines the two commands into one. 

When you run `npm run generate:html:report`, you should see the following output:

<figure>
  <img src="../../images/cypress-report.png" alt="Output of running the cypress generate html report command">
</figure>

## Cypress + Mochawesome Report

Here is a sample report! The report will log the Cypress commands and assertions you use per test and includes details such as overall test execution time, execution time per each test, number of tests passing and number of tests failing.

<figure>
  <img src="../../images/cypress-html-report.png" alt="Cypress and mochawesome report">
</figure>

I've added a clip below to show you what it will look like if you have failing tests.

<figure>
  <img src="../../images/cypress-failed-report.gif" alt="Cypress and mochawesome report with failed tests">
</figure>

I hope this was useful! If you want to look at the example project closely, have a look at my github repo [here](https://github.com/mdcruz/cypress-demo-todo). 🙂
