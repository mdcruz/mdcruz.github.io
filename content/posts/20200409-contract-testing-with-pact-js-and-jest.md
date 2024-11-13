---
author: "Marie Cruz"
title: "Contract Testing with Pact.js and Jest"
date: "2020-04-09"
tags:
- testing
- contract-testing
- tools
- test-automation
---

On my previous blog [post](https://www.testingwithmarie.com/posts/20200331-introduction-to-contract-testing/), I gave an overview about what Contract Testing is and why it's useful for testing microservices. Now let's look at using Pact to show how you can write a contract test.

In this tutorial, I'll be using [pact-js](https://github.com/pact-foundation/pact-js) and [Jest](https://jestjs.io/) but the concept will be similar if you use any of the other Pact supported languages. The sample API that we will be testing today is a simple movie API which allows consumers to retrieve a list of movies from the provider. 

## Installing Pact

Let's first install Pact by running the command below:

`npm i @pact-foundation/pact`

## Consumer Contract Tests

We will be doing consumer driven contract testing so our next step is to write the contract from our consumer side. We need to do the following steps to setup our tests.

1. Require or import the Pact object
1. Create a mock provider
1. Setup the mock provider and register the interactions that it is expecting to receive from the consumer
1. Run the consumer tests
1. Generate the contract

Let's look at steps 1 and 2 first:

```
const { Pact } = require('@pact-foundation/pact');
const path = require('path');

const provider = new Pact({
  consumer: 'Consumer',
  provider: 'Provider',
  port: 4000,
  log: path.resolve(process.cwd(), 'logs', 'pact.log'),
  dir: path.resolve(process.cwd(), 'pacts'),
  logLevel: 'INFO',
});
```

Line 1 requires the Pact object which we will use to create a mock provider. Lines 4-10 sets up the mock provider. We are telling Pact that the name of the consumer is 'Consumer' (not the best name but this is an example!), the mock provider called Provider will run on port 4000, logs (useful for debugging) will be stored on a file called pact.log and the contract will be stored on a folder called pacts. 

Now,  let's look at how we can implements steps 3-5 with the below code snippet.

```
describe('Movies Service', () => {
  describe('When a request to list all movies is made', () => {
    beforeAll(() =>
      provider.setup().then(() => {
        provider.addInteraction({
          uponReceiving: 'a request to list all movies',
          withRequest: {
            method: 'GET',
            path: '/movies',
          },
          willRespondWith: {
            status: 200,
            body: eachLike(
              {
                id: 1,
                name: like('Movie 1'),
                year: like(1999),
              },
              { min: 5 }
            ),
          },
        });
      })
    );

    test('should return the correct data', async () => {
      const response = await fetchMovies(URL, PORT);
      expect(response[0].name).toBe('Movie 1');
      expect(response[0].year).toBe(1999);
    });

    afterEach(() => provider.verify());
    afterAll(() => provider.finalize());
  });
});
```

On my beforeAll hook (line 3), this is where I tell Pact to initialise my mock provider. When this is done, we then proceed in adding interactions to our mock provider (lines 5-25). We are passing an object here which specifies the request that we are registering and what the expected responses will be if we make that request. 

Notice that on the body object, we are saying that the provider should respond to something similar to 'Movie 1' and similar to the year 1999, which is achieved by using a Pact matcher. This means that we don't care what data the provider gives us as long as it's similar to the one we provide. If for some reason, the year 1999 is actually a string in the provider's side, our contract will fail. Also, I have specified that the minimum data the provider should return is 5. 

For my actual test (lines 26-30), I am asserting that when I invoke the actual consumer call to fetch movies that it's returning the data that I am expecting. After the test is run, we then verify the interaction with Pact (line 32). If there are any errors on the interaction that you registered with Pact, this will be logged and the test will fail. Finally, we then create the contract (line 33), upload it to our pacts folder and shut down the mock provider server.

## Publishing the Contract to a Pact Broker

Now that we have a contract, we need to publish the contract to a Pact broker. This way, our provider can run their test and verify that the contract is in line to their agreement. You can host your own Pact broker service but there are also third party software available such as [Pactflow](https://pactflow.io/) that you can use. You can use their free development tier which would allow you to host 5 contracts. Once you have an account created with Pactflow, you can see what your Pact broker url and API token which you will need.

Publishing a contract is easy. I have created a command in my `package.json` file as seen below which will push the contract stored in my pacts folder to the Pact broker. I am passing the consumer version to be 1.0.0, the broker url and the API token (both stored as an environment variable) provided by Pactflow.

`"publish:pact": "pact-broker publish ./pacts --consumer-app-version=1.0.0 --broker-base-url=$PACT_BROKER_URL --broker-token=$PACT_BROKER_TOKEN"`

To simply publish the contract, run:

`npm run publish:pact`

You should see that a new contract has been pushed to Pactflow. Notice that the status of our contract is set to 'Unverified'. This is because the provider has not yet verified the contract from their end. Ideally, before pushing code changes to production, the status of the pact should be set to verified to avoid any unwanted bugs.

<figure>
  <img src="../../images/contract.png" alt="Contract uploaded to PactFlow">
  <figcaption>New contract pushed to Pactflow</figcaption>
</figure>

<figure>
  <img src="../../images/contract-list.png" alt="Contract details uploaded to PactFlow">
  <figcaption>When the contract is viewed, this will list what the agreement is between the consumer and provider</figcaption>
</figure>

## Provider Contract Test

Now we are ready to verify the contract from the provider side. Running the provider test is much simpler than consumer. All we need to do is start our application, pull the contract from the Pact Broker and verify that the contract is correct.

To get started, let's create a test file like the one below.

```
const { Verifier } = require('@pact-foundation/pact');

describe('Pact Verification', () => {
  test('should validate the expectations of our consumer', () => {
    const opts = {
      provider: 'Provider',
      providerBaseUrl: 'http://localhost:3000',
      pactBrokerUrl: process.env.PACT_BROKER_URL,
      pactBrokerToken: process.env.PACT_BROKER_TOKEN,
      publishVerificationResult: true,
      providerVersion: '1.0.0',
      logLevel: 'INFO',
    };

    return new Verifier(opts).verifyProvider();
  });
});
```

First, we need to require the Verifier object from pact (line 1). We then create the options that we need to pass to the Verifier object (lines 5-13) such as the provider name, the provider base url of our application, the Pact broker URL and API token provided by Pactflow. Line 15 is the line of code which runs the provider test and log any errors if the contract is incorrect.

Let's now start up our test provider by running the following command.

`node provider/provider-service.js`

This will load our application on port 3000 and serve a list of example movies.

Now when we run our provider test, if the contract is valid, the status of our contract in Pactflow will update from 'Unverified' to  'Successfully verified'.

<figure>
  <img src="../../images/contract-verified.png" alt="Contract verified in PactFlow">
  <figcaption>Contract is verified and valid 🎉</figcaption>
</figure>

If the contract fails - for example the consumer is expecting the field name to be called movieName instead, then if we run our test, we should see in Pactflow that our verification was unsuccessful.

<figure>
  <img src="../../images/contract-failed.png" alt="Contract verification failure in PactFlow">
  <figcaption>Contract verification failed</figcaption>
</figure>

I have uploaded the complete code in this [repo](https://github.com/mdcruz/pact-js-example). Hope you find this useful! On my next blog post, we'll look at how we can integrate this to a CI pipeline and use tools like the [can-i-deploy](https://docs.pact.io/pact_broker/can_i_deploy) command to help us deploy changes to production safely.