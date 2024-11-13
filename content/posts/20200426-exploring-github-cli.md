---
author: "Marie Cruz"
title: "Exploring Github CLI"
date: "2020-04-26"
tags:
- git
- github
---

Last February, Github announced [Github CLI](https://github.blog/2020-02-12-supercharge-your-command-line-experience-github-cli-is-now-in-beta/), which allows developers to take the actions that they do with Github on their beloved terminals to provide a more seamless workflow. It's still currently on beta mode so the features are still somewhat limited, but enough to get you started. With Github CLI, you can create pull requests and raise issues straight from your terminal.

In this post, I'll be exploring my experience with Github CLI and show examples of the different commands that you can use. 

## Installing Github CLI

Installation is pretty simple. If you are a mac user, all you need is the following command:

`brew install github/gh/gh`

Installation guidelines for Windows and Linux users can be found [here](https://github.com/cli/cli#installation).

If you type `gh` in your terminal, you should now see some available commands that you can run.

<figure>
  <img src="../../images/gh.png" alt="Output of running gh command">
</figure>

Let's look at some of the most common use cases when using Github.

## Creating a new pull request

Without using Github CLI, when you use Git to push a new branch to a remote repo, creating pull requests has to be done on the web app. With Github CLI, we can use the following command:

`gh pr create`

This will run in interactive mode where it will ask you for a PR title and description and next action. In the example below, I selected the Submit action but you can use also select Preview in browser but you would need to have the chrome executable in your $PATH. Also, when adding in the description for the PR, you would have to use nano editor.

<figure>
  <img src="../../images/gh-pr-create.png" alt="Output of running gh pr create command">
</figure>

## Listing all pull requests

To view all available pull requests for your repo, you can run the following command:

`gh pr list`

Here is an example on one of our work projects. If you notice, some of the PR numbers are colour coded to green. This means that these PRs have requested your review.

<figure>
  <img src="../../images/gh-pr-list.png" alt="Output of running gh pr list command">
</figure>

## Viewing PR status

It's great to see an all in one view of information related to pull requests such as a list of PR's that you've opened and list of PR's where your feedback review has been requested. I like that it also shows the PR's status checks whether it's passing or failing. This is handy if you don't often check your emails frequently and you'll know which PRs need your review.

`gh pr status`

<figure>
  <img src="../../images/gh-pr-status.png" alt="Output of running gh pr status command">
</figure>

## Creating a new issue

Similar with creating pull requests, creating a new issue also runs in interactive mode where you provide a title, description and action.

`gh issue create`

<figure>
  <img src="../../images/gh-issue-create.png" alt="Output of running gh issue create command">
</figure>

## Listing all issues

Let's look at all the open issues raised in the Github CLI repo.

`gh issue list`

## Viewing a specific issue

To view a specific issue, you would need to add the issue ID.

`gh issue view 837`

## Thoughts

Github CLI is very easy to install and use. I love that the commands are self explanatory and it definitely speeds up the development workflow. The only thing where I would prefer using the UI is if you are creating pull requests where you have to follow a template description as currently the template description for your repo is not loaded in. As it's still in beta mode, features are limited, as mentioned above so if you are using Github CLI, please share your feedback to the Github team by submitting this feedback form. Also, check out their manual here to know all the current commands.

Thanks for reading! 🙂

--

**Update 28/04/20: Someone from the Github team has kindly reached out and informed that I can set my git_protocol to use SSH so I was able to create a Github repo successfully using the CLI.**


If you are using SSH, you can set this to be the protocol by running the below command:

`gh config set git_protocol ssh`

You can also switch from Nano editor to Vim for example by running:

`gh config set git_editor vim`

Now, to create a new Github repo using the CLI, just simply run:

`gh repo create`