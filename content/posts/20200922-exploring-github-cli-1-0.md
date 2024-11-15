---
author: "Marie Cruz"
title: "Exploring Github CLI 1.0"
date: "2020-09-22"
tags:
- git
- github
---

On one of my previous posts, [Exploring Github CLI](https://www.testingwithmarie.com/posts/20200426-exploring-github-cli/), I talked about Github CLI when it was still on beta mode and have been using its commands on my personal projects. Being able to create a repository straight from the command line or creating pull requests without switching to the Github UI was a time saver. This month, Github announced that Github CLI is officially out of beta mode and since then additional commands have been added in allowing you to run your entire Github workflow straight from your terminal. 

## What can you do with Github CLI 1.0?

With version 1.0, you can now run your entire workflow from your terminal whether it's cloning an existing repository, approving and merging a pull request, creating a new release or creating gists. Version 1.0 also allows you to make authenticated request to Github API directly and also set custom alias command for easy access. Finally, you can now also use Github CLI with Github Enterprise Server.

In this post, let's explore Github CLI 1.0 further and look at the new commands that we can use. To follow along on the commands that I'll be showing below, you need to install Github CLI, which you can find on my previous post linked above. Alternatively, you can also find out more information on how to get started by checking out the [Github CLI official manual page](https://cli.github.com/manual/).

## Authenticating with Github CLI

To authenticate with your Github account, just run the following command:

`gh auth login`

This will run on interactive mode and ask you if it's with a Github account or an Enterprise account and also if you wish to login with a web browser or paste an authentication token which you can get from your Github Settings Token page.

When joining a new project, the first thing that you would typically do is clone an existing repository. Once on the project, you should see an option to clone the project via Github CLI.

When ran on the terminal, you should see the following output:

<figure>
  <img src="../../images/gh-cli.png" alt="Running the command to clone an existing repo with Github CLI produces the same output as cloning it via HTTPS or SSH.">
  <figcaption>Output of running the command to clone an existing project with Github CLI</figcaption>
</figure>

Now that you have the repository clone, the next thing that you probably want to do is create a new branch (please don't ever commit straight to the main branch especially if working with other people!) using Git, work on a feature and push your changes locally on the new branch. When you're ready to create a pull request containing your changes, just run the following command:

`gh pr create`

This will run in interactive mode and will ask you couple of questions such as PR title and description.

Let's now imagine that all checks are passing and it's been approved by your peers. The next step in your workflow is to merge the pull request to the main branch which you can do simply by running the command below.

`gh pr merge`

This will run again in interactive mode and will ask you if you want to do a merge commit, rebase and merge or squash and merge. It will also ask if you want to delete the branch locally as well as on Github! I really like this feature because I'm one of those people who keeps forgetting to do this locally.

## Reviewing Pull Requests
You can also easily review pull requests straight from the command line though with this one, I have to be honest and say that I am more comfortable reviewing it on the Github user interface. But, to do it in the command line, just run the following command on your terminal and it will run again on interactive mode.

`gh pr review`

## Aliasing Github API commands 

This is not really something that I have to do on a daily basis but it's good to know that you can do this easily with Github CLI. Let's imagine that there is an API command that you need to do on a regular basis and rather than making a curl request or using other tools, you can alias the request easily with Github CLI.

For example, this request here will return repositories related to Cypress https://api.github.com/search/repositories?q=cypress 

To make an authenticated API request, simply run the following command:

`gh api /search/repositories?q=cypress`

This should return a JSON object containing repository details related to Cypress.

Now, if you want to refer to it to something more intuitive, we can use Github CLI to set a new alias with the following format:

`gh alias set <alias-name> '<command-to-alias>'`

In our case, it would be:

`gh alias set cypress-repositories 'api /search/repositories?q=cypress'`

Once it's added, just run `gh cypress-repositories` and you should get a similar outcome containing repository details related to Cypress! 🙂

Aliases in Github CLI can also accept parameters. For example, if you want to create an alias to search for repositories depending on what parameter you pass, then this can be easily done as well by running the command below:

`gh alias set searchRepositories 'api /search/repositories?q="$1"'`

Just replaced the value that you want to parameterised with "$1" and when you make a call to this alias, just pass in the value accordingly.

`gh searchRepositories 'cypress'`

As you can see, this can be extended and customisable to your liking.

## Final Thoughts

Github CLI can supercharge your workflow and if you prefer to keep all your development on the terminal directly, then you should definitely check it out. 

For further reading, make sure you check out Github's blog post, [Github CLI 1.0 is now available](https://github.blog/2020-09-17-github-cli-1-0-is-now-available/).