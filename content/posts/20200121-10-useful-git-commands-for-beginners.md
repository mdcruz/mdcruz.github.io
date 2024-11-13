---
author: "Marie Cruz"
title: "10 useful Git commands for beginners"
date: "2020-01-21"
tags:
- git
---

Before I start rambling on what the 10 most useful Git commands are if you're just starting out with Git, let's first talk about what Git is and why it's important for all software developers and QA engineers who are trying to break into test automation to know about it.

Git basically makes it easier for you and your team to track all the changes made on a file. It's a type of version control system which allows you to revisit the changes you have made over time. It shows a log of what changes you did, when you did it and why (via commit messages).  GitHub, on the other hand, is an online service that developers use to manage their code repositories so it can be shared publicly or privately. Git commands are used to push the changes you have made locally to your online GitHub repository. 

Git allows you to clone your team's project locally and make any changes to the source code freely without affecting the work that your team is doing.

This tutorial assumes you have already created a [GitHub](https://github.com/) account and have installed [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) locally. If not, follow the attached links for the installation. To verify that you have installed git correctly, typing `git --version` in your terminal of choice should output the version installed.

## 10 Useful Git Commands for Beginners

1. Initialising a new repository (repo)

`git init`

Say you have an existing project locally and you want to start using version control, this command allows you to initialise a new repo. This also means that all files inside this project will now be tracked for any changes. An indicator that git has been initialised correctly is that a hidden folder called `.git` will now be added to your project.

2. Cloning an existing repo

`git clone <GitHub url>`

Most likely you will join an existing project already to contribute on, in this case, you would need to clone the existing repo from GitHub so you can copy it locally in your machine. The GitHub url will be found on the GitHub repo and you can either clone it by HTTPS or SSH.

<figure>
  <img src="../../images/github.png" alt="GitHub UI for clorning a repo">
  <figcaption>GitHub URL is exposed when you click Clone or download button</figcaption>
</figure>

3. Setting your Git username and email address

`git config --global user.name "your git username"`
`git config --global user.email "your email address"`

This is very useful if this is your first time contributing and you want to set your github username and email address. The parameter `--global` means that this will be your set username and email address globally to all git projects. Omitting this parameter means that this config will be used locally per project. Once this is setup, your team will know what changes you've done as your name and email will be used as the author of your commits.

4. Adding in files ready to be staged 

`git add <files>`

`git add --all or git add .`

When you are finally ready to commit your changes, running git add will stage all the files you have changed and prep it for a commit. In Git, committing your changes are done in two steps. The first step is staging your changes and the second step is actually committing them. It's also quite common practice to just run `git add --all` or `git add .` which basically means staging everything you have changed. When you make more changes to your code again after staging it, you need to run git add again to stage the new changes. 

5. Viewing the status of your project 

`git status`

You might have noticed that when you run git add, nothing gets displayed in the terminal. As you make a lot of changes in your project, running git status displays all the files that you have created, modified or deleted.

It also shows you which files have been staged (files that have been added with git add) or not. The basic colour setup (green means it's staged, red means it's not) is a visual indicator as well to help you understand which files are ready to be committed as shown in the screenshot below.

<img src="../../images/github2.png" alt="Output of running git status">

6. Committing your changes

`git commit -m "your commit message"`

Git commit will save your staged changes to the git directory. Each commit is comprised of a  number called a git hash which allows you to uniquely identify all the changes you've made. Basically, think of git commit like a checkpoint in a game!

If for example, you have a typo in your commit message and you want to update it, you can also do this by passing in the `--amend` parameter after git commit as shown below. 

`git commit --amend -m "your new commit message"`

7. Viewing a history of your commits

`git log`

When you want to view all the commits you have made, this is done by running git log. Git log will sort all commits in chronological order so you always see the latest changes first. Git log will display the commit hash, the author information, the date when the changes were committed and the commit messages itself.

8. Resetting your commits

`git reset HEAD~1`

So, you have committed something but you suddenly realised that you forgot to do one more change in your code or you want to reset all the changes you have done. You want to remove the latest commit like it never happened. In Git, `HEAD` simply means the latest point of your branch (or simply the latest commit). The number after `HEAD~` represents the commit before HEAD. So if you pass, let's say, `HEAD~2`, Git will move its pointer to the second commit before HEAD. 

Git allows you to reset your commits in three ways - soft, mixed and hard reset. If you don't specify a parameter, by default this will used the mixed reset. A mixed reset means that your commit message will be remove from the history and your changes will be untracked.

`git reset HEAD~1 --soft`

A soft reset, like the one above, means that your commit message will be remove from the history but the changes you have made will still be staged. 

`git reset HEAD~1 --hard`

Finally, a hard reset means that your commit message will be remove from the history as well as your changes (it will be like the changes were never there!) so be careful when using this parameter.

9. Connecting your local git repo to a remote GitHub repo

`git remote add origin <url to your github repo>`

You've committed your changes and now you want to push your changes to your GitHub repo. Before pushing your changes remotely, you need to make sure that the repo exists first in GitHub. If you have not created a remote repo yet in Github, visit this link and follow their instructions. Once you have a remote repo, you need to link your local project to the remote repo first before pushing your changes. To do this, you simply need to run `git remote add origin` followed by your GitHub url.

10. Pushing local changes to GitHub

`git push origin master`

Once this is done, you can now push your local commits to the remote repo by running the command above 🎉. When you navigate to your GitHub repo, you should then see the changes that you have pushed.

## Wrapping up

The commands above should help you get started with Git and version control. This is by no means a complete tutorial as there are still a lot of useful Git commands out there. The commands above doesn't consider branching workflows yet so we'll look into that on my next blog post. If you need further reading, I recommend the following resources:

- https://guides.github.com/introduction/git-handbook/
- https://github.github.com/training-kit/downloads/github-git-cheat-sheet/
- https://hackernoon.com/understanding-git-fcffd87c15a3
- https://www.hostinger.com/tutorials/basic-git-commands

