---
author: "Marie Cruz"
title: "Keep Calm and Carry On. It's just a merge conflict!"
date: "2020-02-09"
tags:
- git
---

<img src="../../images/merge_conflict_meme.png" alt="Meme with the text - Merge conflict? Force push when no one is looking">

If you are pretty new with using git or any other version control system tools out there, you'll most likely encounter a scary situation called a merge conflict. Most of the time, git is pretty smart to handle the conflict on your behalf. But there are times when git will need your help and it's these times that some people find scary to do. In this blog post, we'll look at ways on  how to handle merge conflicts and hopefully, by the end of this, you'll be saying "Keep calm, it's just a merge conflict!"

Let's use my experience with my toddler as a way to explain why git needs your help with merge conflicts. So, there are times when my daughter tries to be a responsible adult (threenage stage is real). For example, she puts her socks and shoes on, she brushes her own teeth and she puts her toys away. However, there are also times when she asks for my help because she finds a task too difficult to do! If I am busy with something else, I have to stop whatever I'm doing and prioritise her because I know that if I don't, she will throw a tantrum. This is similar with git! Sometimes, it handles the merge conflicts automatically and sometimes, it will ask for your help because it doesn't want to mess up. When this happens, it can be pretty annoying but it's one of those things that you must do to make sure that the git history is all in order and the appropriate code changes are all in placed.

## Why conflicts happen?

Merge conflicts happen when one or more developer makes changes to the same line of code on the same file. The more changes developers add to the same file, the higher the chance that there will be merge conflicts. It's similar to adding clutter in your room. The more you add clutter and not put things back in order, the messier a room will be.

## Fixing conflicts with Git

In this tutorial, we'll look at using [VSCode](https://code.visualstudio.com/) to fix our merge conflicts. I find that with merge conflicts, it's much easier to do it on an IDE like VSCode or similar rather than doing it directly on the command line. Now, let's look at an example on how to merge conflicts.

My local changes on a txt file has "Test file - commit from local " as its content. Imagine that someone has made changes to the same txt file on the same line remotely which has been merged already. When you try to push your local changes to the remote branch, git will notify you that the update will be rejected, as shown on the screenshot below, because the remote changes are not found on your local branch. 

<img src="../../images/merge_conflict.png" alt="Output of Git local changes getting rejected">

To fix this, you would need to pull the latest changes first. When you run `git pull`, you will see the following message and surprise, surprise there is a merge conflict!

<img src="../../images/merge_conflict2.png" alt="Output of merge conflict">

If we open our text file in VSCode, you can see that the merge conflicts look like annotations. HEAD refers to your latest commit on your branch and any incoming remote changes is annotated by its git commit hash. VSCode has a couple of options for you when it comes to fixing conflicts, you can either:

1. Accept the current changes in your branch if you believe that this is the up-to-date change.
1. Accept the incoming remote changes which will overwrite your local changes.
1. Accept both changes if they are both valid.

<figure>
  <img src="../../images/vscode.png" alt="Git annotations in VSCode">
  <figcaption>Git annotates where the conflicts are and by using an IDE like VSCode, it offers multiple options on how to fix the conflicts easily.</figcaption>
</figure>

Let's go ahead and accept both changes on this one and save our file. When I ran `git status`, I should see something like the image below. What's great about this is that it gives you options to go ahead and commit the merge, or abort the merge if you want to cancel it.

<img src="../../images/git_status.png" alt="Output of running git status">

Once you have committed the conflicts and are happy with the changes, then you are good to push your local branch remotely 🎉

## What happens if you mess up?

This certainly happens to everyone, it happens to me. You're in the middle of fixing conflicts and you've accepted the incorrect changes. It's not the end of the world when this happens. You can restart the process from the start so none of the changes are applied. You have couple of options which you can do if you encounter this situation:

1. If you have pushed your changes remotely to your branch and you want to reset the process, remember from my previous blog post, that you can reset your commit so that it never happens (by passing in the parameter --hard!). By doing a git reset --hard, this removes the commit from the git history. Note however that if you are going to do this, upon pushing your branch remotely, you would most likely need to force push the changes. Force pushing changes overwrites what has been committed already so be careful when using this feature. This can be done by adding the parameter `-f` when running git push.

`git push origin branch-name -f`

2. If you haven't merge the commit yet for the conflict, abort the merge by running the command below and start again from the beginning.

`git merge --abort`

3. If all else fails, ask your fellow developers for help 🙂

If you need further resources on handling merge conflicts with git, check out the following tutorials:

- https://www.git-tower.com/learn/git/ebook/en/command-line/advanced-topics/merge-conflicts
- https://docs.microsoft.com/en-us/azure/devops/repos/git/merging?view=azure-devops&tabs=visual-studio
- https://code.visualstudio.com/docs/editor/versioncontrol#_merge-conflicts





