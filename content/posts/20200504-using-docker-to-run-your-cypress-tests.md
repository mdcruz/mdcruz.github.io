---
author: "Marie Cruz"
title: "Using Docker to run your Cypress tests"
date: "2020-05-04"
tags:
- testing
- tools
- test-automation
- cypress
- docker
---

As a Test Automation Engineer, nowadays you are also expected to contribute in integrating your tests into a build pipeline. As we fully embrace DevOps, Docker has been one of the most popular and most used tool for so many years. When you integrate your tests to a build pipeline, most likely you would have encountered using Docker images or creating a Dockerfile. In this blog post, we’ll look at how we can use Docker to run our Cypress tests. This tutorial assumes that you have a fair understanding of Docker already. But if not, I'll go over some basics. 🙂

## Docker Basics

I am not a Docker expert by any chance! I only know a fair amount to help me with my automated tests. Let's go over some of the basic terminologies to get started.

- Dockerfile - a file that creates a Docker image. Basically, think of it as a set of instructions that you devise to create a Docker image. It contains a set of commands that Docker runs. Like with any instructions, a Dockerfile is executed in order.
- Docker Image - a thing that gets built from your Dockerfile. Basically, it's the end product of your Dockerfile. A docker image can be an existing image (made by other people) or a new image (one that you create). For this tutorial, we will be using the Cypress Docker image.
- Docker Container - a running instance of your Docker Image. In Object Oriented terms, think of Docker image as a class and a Docker container as an instance of that class.

## Pre-requisites

This tutorial assumes you have Docker installed already. If not, please install it from here.

## Cypress Docker Image

The first step is to determine which Cypress Docker image you need to use. Cypress has three categories of images such as:

- cypress/base - image including all Operating System dependencies.
- cypress/browsers - image including all Operating System dependencies and a pre-installed browser (Chrome or Firefox).
- cypress/included - image including all Operating System dependencies as well as the Cypress package.


For this tutorial, I'll be using cypress/included but feel free to use the other images.

## Creating our Dockerfile

On the root of your project, create a file called `Dockerfile`. You can also rename this to whatever you want as long as it has the `.dockerfile` extension.

```
FROM cypress/included:4.1.0

WORKDIR /app

COPY ./cypress ./cypress
COPY ./cypress.json ./cypress.json

RUN npx cypress run
```

Let's walk through the different commands here. First, we are getting the Cypress Docker image from `cypress/included:4.1.0`. We are then specifying that the work directory of our Docker container is called app and then copying the files and folder that we need such as the cypress folder and cypress.json file from our source to the Docker container. Finally, we run the command to run our Cypress tests.

To build the container, simply run with the following command:

`docker build -t cypress-test-image:1.0.0 .`

The parameter `-t` specifies the name of your Docker image. In this example, my image name is cypress-test-image and has the tag version 1.0.0. If you specified a different filename for your Dockerfile, you would also need to pass the name of your file using the parameter `-f`. 

`docker build -t cypress-test-image:1.0.0 -f cypress-test.dockerfile .`

The dot at the end specifies the PATH where Docker should copy the files from. Since I am on the root directory of my project, it will copy the files it need from this location.

When you build the image, you should see an output similar to the following with your Cypress tests running as well.

<figure>
  <img src="../../images/cypress-docker.png" alt="Output of running cypress in docker">
</figure>

To re-run your tests without building the Docker image, simply run:

`docker run -t cypress-test-image:1.0.0 .`

If you want to use the other Cypress Docker images (base or browsers), you would need to update your Dockerfile to also copy package.json and install the project dependencies so Cypress can be installed.

```
FROM cypress/included:4.1.0

WORKDIR /app

COPY ./cypress ./cypress
COPY ./cypress.json ./cypress.json
COPY ./package.json ./package.json

RUN npm i && npx cypress run
```

## Real work scenarios where Docker can help

I've noticed that in our builds, we are seeing this [issue](https://github.com/cypress-io/cypress/issues/5965) frequently which could be related to an issue with the Electron browser. One of the suggested comments is to run the tests headlessly on Chrome instead of Electron. To try it, I updated the Cypress Docker image that we are using to this one [here](https://github.com/cypress-io/cypress-docker-images/tree/master/browsers) depending on which browser version, node version, OS etc we want. The setup is still pretty much similar, the only changes that I've done is to change the Cypress image on our Dockerfile and also updated the test command that we used to run it headlessly on Chrome.

To run your Cypress tests headlessly, you just need to pass the following parameters. For example:

`npx cypress run --headless --browser chrome`

Another scenario where Docker has helped us is with visual testing. As we are currently doing visual tests to support one team that we are working with, we are experiencing an issue where the baseline and test images always have a lot of mismatch. This is because we generate the baseline images on our local machine whereas the test images are generated on our CircleCI pipeline which is running on a different machine. By using the Cypress Docker image to generate the baseline images, we have seen less mismatch errors on our visual tests since the tests are run on the same Operating System, node version etc.

## Additional Resources

- [Beginner's guide to Docker](https://dev.to/peterj/beginners-guide-to-docker-4pkk)
- [Run Cypress tests on Docker using a single command](https://www.cypress.io/blog/2019/05/02/run-cypress-with-a-single-docker-command/)
- [Run Cypress tests on Docker](https://medium.com/@zite/so-you-want-to-get-cypress-running-in-docker-7e8fb2837731)