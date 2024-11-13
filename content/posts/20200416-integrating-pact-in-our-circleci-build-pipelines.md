---
author: "Marie Cruz"
title: "Integrating Pact in our CircleCI build pipelines"
date: "2020-04-16"
tags:
- testing
- contract-testing
- tools
- test-automation
---

After following my previous blog [post](https://www.testingwithmarie.com/posts/20200409-contract-testing-with-pact-js-and-jest/), hopefully by now you have a solid foundation about what Contract testing can offer. In one of the projects that I am involved with, we are trying to do contract testing so I need to understand the different features that Pact can offer. In this post, we’ll look at how we can integrate our contract tests to a CI/CD pipeline and how we can use versioning and tags whenever our consumers and/or providers update the contract. For this tutorial, I'll be using CircleCI and the Pact broker CLI command.

## Integrating Consumer tests on CircleCI
Let's first start by integrating our consumer tests to CircleCI. CircleCI has a great getting started guide which you can use to create your first build pipeline. Just follow the instructions along and you should be able to setup a pipeline in a short time. All you need is the github repo where your tests are stored. Feel free to fork [mine](https://github.com/mdcruz/pact-js-example) as an example. I created a different branch which contains a `config.yml` file, like the one shown below, to be stored in the root `.circleci` folder.

```
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:latest
    steps:
      - checkout
      - run: 
          name: Install project dependencies
          command: npm install
      - run:
          name: Run consumer contract tests
          command: npm run test:consumer
```

Ok, let's walk through this config one by one. We are using CircleCI version 2.1 and we have defined one job called build. A job in CircleCI is simply a collection of commands or tasks that CircleCI runs. In this job, we are using the Docker command to get the latest image of Node so we can run the commands that we have to run our tests. After we check out our code, we run `npm install` to install our project dependencies and `npm run test:consumer` which runs our consumer contract tests.

Let's push these changes to a new branch and if there are no issues, it should pass as expected.

<figure>
  <img src="../../images/circleci.png" alt="Passing checks in GitHub">
</figure>

<figure>
  <img src="../../images/consumer-test.png" alt="Output of running consumer contract tests">
</figure>

What's great about CircleCI is you don't need additional installations to get started. Just create a `.circleci` folder in your github repo, add your `config.yml` file and you can pretty much get started already. Now, let's update our CircleCI config to add another step to upload our defined contract to our Pact broker.

## Uploading Contract to Pact Broker

Since we are using environment variables to store our Pact broker URL and API token, we need to store this in our CircleCI project settings.

To create environment variables in CircleCI, we can follow the instructions [here](https://circleci.com/docs/2.0/env-vars/#setting-an-environment-variable-in-a-project). I stored `$PACT_BROKER_URL` and `$PACT_BROKER_TOKEN` as shown below.

<figure>
  <img src="../../images/env-vars.png" alt="Environment variables in CircleCI">
</figure>

Now, let's update our CircleCI config to have a step for setting our environment variables and another step to publish our contract.

```
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:latest
    steps:
      - checkout
      - run:
          name: Setup environment variables
          command: |
            echo "export PACT_BROKER_TOKEN=$PACT_BROKER_TOKEN" >> $BASH_ENV
            echo "export PACT_BROKER_URL=$PACT_BROKER_URL" >> $BASH_ENV
      - run: 
          name: Install project dependencies
          command: npm install
      - run:
          name: Run consumer contract tests
          command: npm run test:consumer
      - run:
          name: Publish contract to Pactflow
          command: npm run publish:pact
```

I added a step to setup the environment variables before installing my project dependencies. CircleCI uses bash so it automatically loads all environment variables stored in `$BASH_ENV`. I am simply exporting my `PACT_BROKER_TOKEN` and `PACT_BROKER_URL` to `BASH_ENV` so it can be accessed by my other steps later on.

The second command that I added is to publish the contract to Pactflow by running `npm run publish:pact`. Since the environment variables are loaded already, the step should pass as expected.

<figure>
  <img src="../../images/publish-contract.png" alt="Publishing a contract to PactFlow">
</figure>

## Running Provider test on CircleCI

Normally, your provider will be on a separate project. But for this example, the provider tests are also on the same project repo (again for simplicity). Since I have already stored the environment variables that I needed, the only thing that I need to do is to add another step on my build job to run the provider test. `npm run start:and:test:provider` in this case does 2 things. First, it starts my test provider service and then the actual provider test is run.

```
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:latest
    steps:
      - checkout
      - run:
          name: Setup environment variables
          command: |
            echo "export PACT_BROKER_TOKEN=$PACT_BROKER_TOKEN" >> $BASH_ENV
            echo "export PACT_BROKER_URL=$PACT_BROKER_URL" >> $BASH_ENV
      - run: 
          name: Install project dependencies
          command: npm install
      - run:
          name: Run consumer contract tests
          command: npm run test:consumer
      - run:
          name: Publish contract to Pactflow
          command: npm run publish:pact
      - run:
          name: Run provider contract tests
          command: npm run start:and:test:provider
```

<figure>
  <img src="../../images/contract-provider.png" alt="Output of running provider contract tests">
</figure>

All seems well, the contract is getting verified by the provider. However, we still have one issue here. On the real world, our consumer and provider will have different versions to mark that they have introduce small or breaking changes. We need to be able to detect in our contract tests that we are running it on the version that we want. Basically, we need to ask ourselves this question before we deploy our changes to production - *"is it safe for me to deploy this consumer version or provider version to production?".* This is where the command [can-i-deploy](https://github.com/pact-foundation/pact_broker/wiki/can-i-deploy) comes in which comes built in with the Pact broker CLI tool.

## Can I Deploy?

The Pact broker records the consumer version whenever a contract has been uploaded. At the same time, when the provider verifies the contract, the provider version is also recorded by the broker. We then end up with a table called the "Pact Matrix" which holds different consumer and provider versions and if the contract was verified successfully or not on each of these versions.

<figure>
  <img src="../../images/pact-matrix.png" alt="Pact matrix">
  <figcaption>Pact Matrix is built in on your Pact Brokerw</figcaption>
</figure>

I slightly modified my command to publish the contract by using the Git commit SHA of my last commit instead of hardcoding the value. The environment variable $CIRCLE_SHA1 is a built-in variable from CircleCI.

`"publish:pact": "pact-broker publish ./pacts --consumer-app-version $CIRCLE_SHA1 --broker-base-url $PACT_BROKER_URL --broker-token $PACT_BROKER_TOKEN"`

Now, let's used the can-i-deploy command to see if we can deploy our latest consumer version with the latest provider version, which I will hardcode as 1.0.0 for now.

`"can:i:deploy:consumer": "pact-broker can-i-deploy --pacticipant Consumer --version $CIRCLE_SHA1 --pacticipant Provider --version 1.0.0 --broker-base-url $PACT_BROKER_URL"`

**Note: pacticipant is not a typo! this is the actual name that Pact uses!**

Let's run this command by typing:

`npm run can:i:deploy:consumer`

<figure>
  <img src="../../images/can-i-deploy.png" alt="Output of running can-i-deploy">
</figure>

All seems good! However, we are hardcoding the provider version. Most likely, we won't have an idea what the provider version will be. We can ask them of course but that would mean our solution will not scale correctly. An alternative is to use tags. Whenever a provider successfully verifies the contract, we can add another step on the provider pipeline afterwards to create a version tag and push this tag to our Pact broker. For example:

`"create:provider:tag": "pact-broker create-version-tag --pacticipant Provider --tag staging --broker-base-url $PACT_BROKER_URL"`

Now, we can amend our command on the consumer side so we just need to verify if it's safe to deploy our consumer version to the provider's latest staging tag. For example:

`"can:i:deploy:consumer": "pact-broker can-i-deploy --pacticipant Consumer --version $CIRCLE_SHA1 --pacticipant Provider --latest staging --broker-base-url $PACT_BROKER_URL"`

We can amend the tags to be whatever but ideally, it should match our environment names (dev, staging, prod) so it's clearer. 

## Putting it all together

My CircleCI config now looks like the one below:

```
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:latest
    steps:
      - checkout
      - run:
          name: Setup environment variables
          command: |
            echo "export PACT_BROKER_TOKEN=$PACT_BROKER_TOKEN" >> $BASH_ENV
            echo "export PACT_BROKER_URL=$PACT_BROKER_URL" >> $BASH_ENV
      - run: 
          name: Install project dependencies
          command: npm install
      - run:
          name: Run consumer contract tests
          command: npm run test:consumer
      - run:
          name: Publish contract to Pactflow
          command: npm run publish:pact
      - run:
          name: Run provider contract tests
          command: npm run start:and:test:provider
      - run:
          name: Tag latest provider version
          command: npm run create:provider:tag
      - run:
          name: Can I deploy consumer?
          command: npm run can:i:deploy:consumer
      - run:
          name: Tag latest consumer version
          command: npm run create:consumer:tag
```

Obviously, in real world examples the provider pipeline is separate from the consumer pipeline. But hopefully, this will give you an idea on how to use the Pact broker and integrate it in your pipeline. Ideally, whenever a consumer pushes a new contract, a notification should be sent to the provider so the provider verification tests are run. When the provider tests are run and successful, it should send a notification back to the consumer so the consumer can then deploy their changes safely. 

<figure>
  <img src="../../images/contract-flow.png" alt="Contract testing flow">
</figure>

I found the above image helpful from Pact's official documentation to visualise what our pipeline should look like when everything is integrated together. This can be achieved by using [webhooks](https://docs.pact.io/pact_broker/advanced_topics/webhooks) which I'll play around with next time 🙂

Github repo for this example: https://github.com/mdcruz/pact-js-example