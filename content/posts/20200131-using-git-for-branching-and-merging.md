---
author: "Marie Cruz"
title: "Using Git for branching and merging"
date: "2020-01-31"
tags:
- git
---

On my previous [post](https://www.testingwithmarie.com/posts/20200121-10-useful-git-commands-for-beginners/), I talked about the 10 useful basic Git commands for beginners. While these commands are beneficial, there are additional commands that you need to know to cover branching workflows for collaborating with your team effectively. Most likely, when you work on your first team programming project, you will see a lot of different branches containing codes from different people. You'll encounter the term `pull requests (PR)` and `peer reviews`. You will also notice immediately that when you try to commit your changes directly to a branch named master, you will be frowned upon! Why? Here’s why.

The master branch is the branch that everyone in your team considers production ready, the branch that you shouldn’t mess about! Basically, if you want to make a good first impression, do not commit your changes straight to master branch, unless you work alone! Your code needs to branched off master so you don't interfere others with your work, which is one of the focal point as to why teams use version control systems. Let's use a tree as analogy as to how a branching workflow works.

Think of the master branch as the central trunk of your tree. When you want to introduce a new feature or a bug fix, you branch off the trunk. A branch is not part of the trunk of the tree but is connected to it. Similar with Git, a branch is an extension of master. You can have as many branches as you want and a branch could even be extended from another branch. At some point, these branches will need to merge back to master so it can be deployed. A basic branching strategy that is most widely used is called the 'Github flow'. With this flow, you create a branch and open a pull request so it can be reviewed by your team and once your feature is approved, your branch will be merged to master branch. 

## Git commands for branching

To simply create a new branch, run:

`git checkout -b your-branch-name`

The branch name can be anything but most teams will have a naming convention to follow. When you run this command, you also switch to this new branch automatically. Any changes you make on this branch will not affect master branch or any other branch.

If you want to list all the branches you have locally, you can run:

`git branch`

To switch to another branch, it's similar to creating a new branch but you omit the `-b` from your command. If you try to switch to a branch that does not exist, Git will throw an error.

`git checkout another-branch-name`

To delete a branch that you no longer need, you can run the following command. Note that you cannot delete the branch that you are currently on.

`git branch -D your-branch-name`

To rename your branch, you need to run the following. With this command, you can rename the branch that you are currently on.

`git branch -mv old-branch-name new-branch-name`

Once you are happy with the changes you have made on your branch and you have committed the changes, to push your branch to Github, you need to run:

`git push origin your-branch-name`

When it's successful, you should see something similar like the image below. When you click the link that is provided by Github, this will redirect you to a page where to submit your pull request, ready to be reviewed by your team.

<img src="../../images/github3.png" alt="Output of running git push">

## Git commands for merging

As a rule of thumb, if you work in a team that merges changes to master branch frequently, it's a good practice to always keep your branch sync with master branch to avoid a state of fixing a lot of merge conflicts. If you need to pull the latest changes from the remote master branch, you checkout master and then run the following command:

`git pull`

This command pulls all the changes from the origin Github remote repo, and applies all the remote changes to your local master branch. To actually merge these latest changes to your feature branch, you checkout your branch again and merge the latest changes in by running:

`git merge master`

After running this, you might see the following screenshot which prompts you to add a commit message. Most of the time, the commit message that it provides is sufficient already and you just need to save the changes either by executing `:x` or `:wq` in your terminal.

<img src="../../images/github4.png" alt="Output of running git merge">

Git merge will apply the changes that have been committed from master branch on top of your changes. On the other hand, if you want your changes to be applied on top of what has been committed already on master branch, you run the following command.

`git rebase master`

The only difference with git merge and git rebase is the way your git log or history will show. With git rebase, your changes will appear as the latest commit. Some prefers one over the other. Personally, I prefer doing a rebase.

There will be times when you have committed something to your branch and your changes have been merged to the remote master branch in Github. For whatever the reason is, you and your team have decided that the changes need to be reverted. To revert a commit, run the following command followed by the commit's hash number.

`git revert commit-hash-here`

With the above command, a new commit will be made which reverts the changes you want as shown on the screenshot below. 

<img src="../../images/github5.png" alt="Output of running git revert">

This preserves the history of all your commits which is the safest and recommended way! There are definitely ways to rewrite the commit history and you can even force delete some of the commits but this is bad practice in my opinion since the traceability of any changes made in the past will be removed. Plus in the future, if you want to do a revert of the revert of the revert then this is all possible. 😅
 
On the next blog post, we'll look at handling merge conflicts, which most people find scary to do. If you need further reading regarding branching and merging, I recommend the following resources:

- https://docs.microsoft.com/en-us/azure/devops/repos/git/git-branching-guidance?view=azure-devops
- https://guides.github.com/introduction/flow/
- https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

