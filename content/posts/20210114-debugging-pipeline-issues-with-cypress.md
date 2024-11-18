---
author: "Marie Cruz"
title: "Debugging Pipeline Issues With Cypress"
date: "2021-01-14"
tags:
- testing
- tools
- test-automation
- cypress
---

Recently, I have migrated our Cypress version to version 6 so we can make use of its latest features. While doing the migration, I've experienced the classic scenario where everything was working fine locally. So I celebrated early, made myself a cup of tea, then the minute I pushed up my changes to our Continuous Integration pipeline, something was wrong and my build was failing! 

So I figured, I'll write up a blog post about it so I can look back at something that I've written in the future in case I encounter the same issue again. Hopefully it can help someone as well who's experienced the same issues.

## Missing Cypress Binary

The issue that I encountered was about a missing Cypress binary. To give you a background of our build pipeline, we have a separate CircleCI job for installing the dependencies and then a separate job for running the actual tests. If you only have one CircleCI job which combines the steps to install dependencies and run the tests, then you wouldn't encounter this issue but in most work projects, you tend to separate these jobs to make it more distinct and easy to extend. To demonstrate how to cache the Cypress binary between different jobs, I created an example [Cypress CircleCI github repository](https://github.com/mdcruz/cypress-circleci-example/pull/1/files) which you can use for reference.

The simplest way to fix this is to make sure that you are caching the `~/.cache` folder where the Cypress binary is located. You can find a snippet of the CircleCI config that I created on the example repository.

```
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
```

The key thing to know here is the `save_cache` key and notice that I am caching both `.npm` and `.cache` to cache both the node modules and the Cypress binary. Also notice that I am giving it a unique key value. The key can be anything but this represents our fingerprint for this cache. This will help in speeding up our build pipelines! Basically, if the package dependencies for your project have not changed, the key will not change so it will restore the downloads from the cache instead. 

The next thing to do is to also add the `restore_cache` key in your other job which runs the Cypress tests using the same key value that you set previously.

```
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
```

In most cases, this should fix the missing Cypress binary issue!

However, in our specific case, we were already caching the `.cache` folder. After upgrading to version 6, I was confused and scratching my head when I got this error that "The cypress npm package is installed, but the Cypress binary is missing".

The next fix that I found was to set the environment variable `CYPRESS_CACHE_FOLDER`. 

According to this [Cypress installation guide](https://docs.cypress.io/guides/getting-started/installing-cypress.html#Binary-cache):

"Cypress downloads the matching Cypress binary to the global system cache, so that the binary can be shared between projects."

By default, global cache folders are:

- MacOS: ~/Library/Caches/Cypress
- Linux: ~/.cache/Cypress
- Windows: /AppData/Local/Cypress/Cache

In our specific case, it's expecting the Cypress binary to be installed on a different folder due to the way we set up our directory structure. So to fix it, I had to override where I want the binary to be located to `~/project/.cache/Cypress` and then set this environment variable on both my jobs. Of course, this will vary depending on your working directory but the error message that you get from your build should give you a hint.

Bonus tip, by using `CircleCI's Anchors and Aliases`(https://circleci.com/docs/2.0/writing-yaml/#anchors-and-aliases), you can keep the config file DRY as you can see from the code snippet below.

```
cypress_defaults: &cypress_defaults
  working_directory: ~/project
  environment:
    CYPRESS_CACHE_FOLDER: '~/project/.cache/Cypress'
    
install_deps:
	<<: *cypress_defaults
    
run_tests:
	<<: *cypress_defaults
```

## Missing libgbm Dependency

A colleague of mine was experiencing this issue and I learned that as of Cypress version 5, `libgbm-dev` is now a requirement dependency if you want to run Cypress on a Linux machine. 

There seems to be two easy fixes for this. The first fix is to manually install the required dependencies which you can copy from this [Cypress linux requirements](https://docs.cypress.io/guides/getting-started/installing-cypress.html#Ubuntu-Debian) documentation. The other fix, which I personally think is much better, is to just upgrade the Cypress docker image that you are using. Most of the recent images now contain this dependency.

## Wrapping Up

There you have it! I've shown you how I managed to fix some of the pipeline issues I've experienced lately with Cypress. And... just remember that just because it works locally, doesn't mean that it will work in your pipelines! 😂