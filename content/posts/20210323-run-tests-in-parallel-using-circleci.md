---
author: "Marie Cruz"
title: "Run Tests in Parallel using CircleCI"
date: "2021-03-23"
tags:
- testing
- tools
- test-automation
- cypress
- continuous-integration
---

One of the strategies out there to speed up your tests is to run them in parallel especially if you have a large codebase with lots of tests! Fast feedback loop is crucial to speed up development work and if you have automated tests running for a long time, it slows down everyone's work. In this post, I would like to quickly show you how to run your tests in parallel using [CircleCI](https://circleci.com/).

CircleCI has a feature to split your tests in parallel by executing them on multiple executors. You can read more about this feature in their documentation, [Running Tests in Parallel](https://circleci.com/docs/2.0/parallelism-faster-jobs/). In this specific example, I will show how to split your Cypress tests but this will apply to other tests too. This tutorial will assume you are using a paid version of CircleCI as the free version only includes 1 container to execute from. Of course, if you are using the Cypress Dashboard already, then you don't need to follow along. 😀

## Initial CircleCI config file

My initial CircleCI config file looks like the one below. Without going into too much technical detail, I have set up two jobs. One to install the project's dependencies and the other to run the tests. These jobs are then called sequentially on a single workflow called `build_and_test`.

```
version: 2.1

jobs:
  install_dependencies:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - restore_cache:
          key: npm-cache-{{ checksum "package.json" }}
      - run:
          name: Install project dependencies
          command: npm ci
      - save_cache:
          key: npm-cache-{{ checksum "package.json" }}
          paths:
            - ~/.npm
            - ~/.cache

  run_cypress_tests:
    docker:
      - image: cypress/browsers:node12.18.3-chrome87-ff82
    steps:
      - checkout
      - restore_cache:
          key: npm-cache-{{ checksum "package.json" }}
      - run:
          name: Running cypress tests
          command: npm test
      - store_artifacts:
          path: cypress/videos
      - store_artifacts:
          path: cypress/screenshots

workflows:
  build_and_test:
    jobs:
      - install_dependencies
      - run_cypress_tests:
          requires:
            - install_dependencies
```

## Set up parallelism

Let's first setup the parallelism level on your config file. This parallelism key defines how many independent executors CircleCI will create to run your tests. To do this, you just need to add this key before calling the steps to run your tests. Depending on your CircleCI's pricing plan, the parallelism value can be increased to your requirements.

```
run_cypress_tests:
  docker:
    - image: cypress/browsers:node12.18.3-chrome87-ff82
  parallelism: 4
  ```

Easy right? That's the first step! Now in order to split the actual tests, the next step that we need to do is use CircleCI CLI to call additional commands to split our tests.

## Split Tests using CircleCI CLI

The commands that we will use are `circleci tests glob` and `circleci tests split`. If you are not familiar with glob, a glob is basically a pattern that will match a list of filenames to a specific pattern that you specify.

The split command by default will split your tests according to a list of filenames but you can also use other strategies such as splitting it by timing data or file size. For this tutorial, I will use the default way.

By using these two commands, you can split your tests into multiple executors that are independent of each other as you can see from the image above.

Now, let's look at how we can achieve this in our configuration file.

```
- run:
    name: Running cypress tests
    command: |
      mkdir cypress/tmp
      mv $(circleci tests glob cypress/integration/examples/*.spec.js | circleci tests split)
      cypress/tmp || true [ "$(ls -A cypress/tmp)" ] && npm test
```

There's a lot going on at the above code so I will explain it line by line.

First, we create a new temporary directory called `cypress/tmp`. This is needed because this is where the files that are split by CircleCI will be stored. 

Next, using the glob command, I am trying to match all filenames that follows the pattern `cypress/integration/examples/*.spec.js` and then we are piping the output to `circleci tests split`. Wait, piping? what is that?! Piping basically means you are redirecting the output to another command.

CircleCI will then do its own magic and split the tests and then we'll move the split files to the `cypress/tmp`  folder. For this to work, you need to modify your Cypress configuration to point at this specific folder when you run the tests. 

Then finally, just call the command that you use to run your tests. To avoid scenarios where the executors don't have tests to run, I added an error handling command that will check if the `cypress/tmp` folder is empty. If it's not empty, then continue and run the tests and that is pretty much it! 🎉

## Wrapping up

In this post, I showed one way of running tests in parallel using CircleCI. Applying test parallelism regardless of the technical approach is always great news because it will speed up your build times and even in most cases, cut the times nearly half.

For additional resources on how others have used the parallelism feature, you can check out the following blogs:

- [How to boost build time with test parallelism](https://circleci.com/blog/how-to-boost-build-time-with-test-parallelism/)
- [Reducing App Testing Time Using Jest and CircleCI](https://betterprogramming.pub/reducing-app-testing-time-using-jest-and-circleci-39170231fee2)

