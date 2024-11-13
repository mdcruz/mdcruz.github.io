---
author: "Marie Cruz"
title: "Using AWS SNS and CloudWatch API for test automation"
date: "2019-11-27"
tags:
- testing
- tools
- test automation
---

One of the areas that we are currently responsible for testing is the cache clearance service whenever articles are created, updated or deleted. To provide some high level overview as to how our cache clearance service is built, whenever editorials create or update articles, a notification is sent to our AWS SNS (Simple Notification Service) which will then trigger the various lambdas that we have. These lambdas then proceed in doing what they are supposed to do to clear the articles and the logs are then sent to AWS CloudWatch. Finally, the same logs are exported to DataDog. 

The current test scenarios that the previous QA team has handed over involve doing the end-to-end workflow of manually creating test articles, verifying either in DataDog or CloudWatch that there are no errors in the log and then verify if the updates are reflected as expected on the front end. While it's important to test the end-to-end workflow to ensure that the whole integration works, it also takes a lot of time and effort to execute them manually and can slow down the release process whenever there is a new change required to be deployed.

Last week, I was working on a ticket to spike how we can automate our cache clearance service. Initially, I was thinking of doing the automation via making calls to the Wordpress API which would trigger the lambdas that are responsible for clearing the cache and then making API calls to DataDog to verify that the logs for a particular article ID returns no errors and has the correct log information. If there are no errors in the log, then this is a good indicator that the cache has been cleared. While this is doable, this test heavily relies on the Wordpress API and if this service is down for whatever reason, this test will also fail. It was a good thing that one of our developers (Thanks Jakub! 🙂) had pointed out that there is an alternative way to trigger the lambdas by sending a message to AWS SNS programmatically, bypassing Wordpress API overall.

To publish a message to SNS programatically, the below code snippet can be used. Note that this tutorial will be using AWS SDK for JavaScript. For further information regarding setup, please refer to this [documentation](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-nodejs.html).

```
const AWS = require('aws-sdk');
const sns = new AWS.SNS({ region: 'eu-west-1' });

// Create your message params
// Message is a required value. This can be in the form of XML, JSON or in this example a string
var params = {
  Message: 'Hello World', /* required */
  TopicArn: 'arn:aws:sns:eu-west-1:123456789012:your-ARN-topic-here'
};

// Publish the params to SNS
const publishTextPromise = sns.publish(params).promise();

// Handle the promise's fulfilled and/or rejected state
publishTextPromise
  .then(data => {
    console.log(`Message ${params.Message} send sent to the topic ${params.TopicArn}`);
    console.log('MessageID is ' + data.MessageId);
  })
  .catch(function(err) {
    console.error(err, err.stack);
  });
```

The values that you pass in your params are the important bits here. Message is the actual payload that you send to SNS. Line 10 publishes this to SNS and we then handle the fulfilled state (publish to SNS successful) and/or rejected state (publish not successful) of the promise.

Once the message has been published successfully, the next step is to check CloudWatch logs to verify that the lambdas have been triggered successfully. This can be done by using the code snippet below. 

```
const AWS = require('aws-sdk');
const cloudwatchLogs = new AWS.CloudWatchLogs({ region: 'eu-west-1' });

async function getCloudWatchLogs() {
  const timestamp = new Date();
 
  // Create message params. endTime, queryString and startTime are required fields here
  // queryString follows CloudWatch Insight Query syntax. 
  // More info here https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html
  const params = {
    endTime: timestamp.getTime(), /* required */
    queryString: `fields @message 
    | sort @timestamp desc
    | limit 20`,  /* required */
    startTime: timestamp.setHours(-1),  /* required */
    logGroupName: 'Your log group name here'
  };
  
  // Start the query. When we start a query, this returns a queryId for us to use on our next step.
  const data = await cloudwatchlogs.startQuery(params).promise();
  const { queryId } = data;
  return waitForQueryResults(queryId);
}
 
// This function waits for the query results and logs the results when found
function waitForQueryResults(queryId) {
  console.log(`Query id: ${queryId}`);
 
  async function checkResults() {
    cloudwatchlogs.getQueryResults({ queryId }, async (err, data) => {
      // This check is needed because it takes some time for the query to appear. 
      // Status will go from 'Running' to 'Complete' when it's finished
      if (Array.isArray(data.results) && data.status === 'Complete') {
        console.log(data.results);
      } else {
        console.log('Logs not found. Retrying...');
        return checkResults();
      }
    });
  }
  return checkResults();
}

await getCloudWatchLogs();
```

There are two steps here that we need to do. First we need to start a query. But before doing that, we need to specify what our params would be. startTime, endTime and queryString are all required values here so make sure that these are provided. Also, if you need to verify different log groups, you can substitute logGroupName with logGroupNames and pass the different log groups as an array of strings. Line 20 starts the actual query and if successful, this will give us a query ID to use. Lines 26-42 is a recursive function that you can use which gets the actual query results. If it has finished scanning all the logs, the status of the run will be set to 'Complete'. When it's complete, you would also need to verify that the results are returned as AWS CloudWatch will report that it's complete but results are not returned yet. Finally, we then print the logs in our console.

Now that we have the data we need, all we need to do is to use a testing framework like Jest to assert that there are no errors in the log and assert that the log information is correct. Going forward, this will save us huge amount of time and effort as we won't need to verify the logs manually. In our case, that's 6 different log groups. We will still need to test the entire workflow manually by picking 1 or 2 scenarios but at least now we have a way of automating the laborious tasks of verifying each of the lambda logs.

## Resources

- https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-nodejs.html
- https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sns-examples-publishing-messages.html
- https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_StartQuery.html
- https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetQueryResults.html