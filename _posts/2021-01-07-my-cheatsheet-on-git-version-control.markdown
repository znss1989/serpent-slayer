---
layout: post
title:  "My Cheatsheet on Git version control"
author: Wu, Lei
date:   2021-01-07 10:21:00 +0300
categories: development version-control
tags: [git, version-control]
---

This article is my summary and cheatsheet in practice for Git usage and version control in general. It may be updated with more details along the way.

## Git for version control

Version control is one of the most commonly used techniques in almost any practical modern software projects, especially when collaboration among different team members are required. [Git](https://git-scm.com/) and its related ecosystem are the most popular set of tools of version control adopted by developers, thus have become a must-learn nowadays.

Most simply put, version control allows tracking of all changes in a software project over time, which can be helpful when one wants to revert and work based on a previous version of a project, and when multiple people are working on different features of a project. Handling such records of changes manually can be a nightmare due to complexity, and it is prone to errors.

## Git at local

According to its official description:

> Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

For this distributed version control to work, Git keeps the complete records of all changes that take place over time in a project (or codebase and repository interchangably). And the state of the codebase at a particular point of time is called a snapshot in Git terminoloty, some other version control systems works in different manners, say keeping changes between versions instead. 

This makes adding new changes, checking or moving pack to previous changes both fast and easy. The same above will apply to each of the local repository of the contributors of the same project, and works similarly on the remote server, e.g. on [GitHub](https://github.com/).

Once tracked by Git, there are three possible states for files in a Git repository to be, namely *modified*, *staged*, and *committed* (or unmodified).

![Git state lifecycle](https://git-scm.com/book/en/v2/images/lifecycle.png)

### Set up

To begin using Git, it would be convenient to configure it with proper credentials, e.g. username and email, so that source code changes and other kinds of contributions in a project can be identified easily. This can be done on the local level, global level and system level, with increasing scope.

A sample configuration could be the following.

```bash
git config --global user.email "znss1989@gmail.com"
git config --global user.name "Wu, Lei"
```

To check those values, the commands below can be used.

```bash
git config user.email
git config user.name
git config --list
```

With the credentials configured properly, now it is time to tell Git to start tracking in a working project.

```bash
mkdir demo && cd demo
git init
```

The command `git init` simply creates an empty project, with a `.git` directory dealing with all the work for tracking. Do check its content if feel interested in how Git works. However, nothing will be tracked automatically at the moment, until explictly told such.

### Single chain

A series of changes in a project will be put by Git as one chain of snapshots, each of which as mentioned above is a complete record of the whole project at a particular point of time. The most recent one of the snapshot chain is called `HEAD`. The command `git add` will start the tracking of contents in a project. 

```bash
git add <file>
```

To track all the contents, wild cards `.` or `*` can be used.

```bash
git add .
```

To verify that whether Git is indeed working as expected, `git status` can be used to check which files are modified, staged and so on.

```bash
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   foo
```

As can be seen above, file `foo` is now staged by Git, which can be commited later into a snapshot. And command `git rm --cached <file>` can be used unstage files into untracked state. A commit (snapshot) can be made by the `git commit` command, as below.

```bash
git commit -m '<description_of_commit>'
```

With changes made and tracked in a project, it is possible to use `git log` command to view the commit history for your project, sorted reversely by the creation time.

```bash
git log --oneline
```

Option flag such as `--oneline` helps format the information in an easier-to-read format.

Most likely, one would make mistakes during the process of making these commits of changes. Command `git reset` is helpful fixing such errors.

```bash
git reset --soft HEAD~1
```

Here the `--soft` option turns the committed contents into staged area. Note that the number following `HEAD~` denotes the number of commits to revert. If the contents are not needed at all, `--hard` flag can be used instead.

### Branches

Git based version control allows parallel works going simultaneously, like development on different features while fixing bugs or polishing documentation of existing version at the same time. This is achieved by branches in Git, which avoids the confusion caused by mixing changes of multiple works together. 

By default, there is a primary branch called `master`. When creating another branch `git branch <new_branch>`, Git actually creates a new pointer to the current snapshot. 

The command `git branch` will list out all the branches in a repository, and indicating the current branch with the asterisk `*`. Making commits will only update the current branch that is worked on. And `git checkout <branch_name>` allows to switch into another branch that is present. A shortcut to create a new branch and switch to it at the same time is the command `git checkout -b <new_branch>`.

To rename an existing branch, `git branch -m <new_branch_name>` or `git branch -m <old_branch_name> <new_branch_name>` can be used. In similar manner, `git branch -d <branch_name>` will delete an existing branch.

When working across different branches, sometimes there is need to keep some active work temporarily, which is not completed and ready to be committed, before switching to another branch. `git stash` will come to the rescue for such cases, by storing the staged and modified contents in cache, all the while making the working branch directory clean.. After switch back to the branch that is stashed, `git stash apply` will get the temporary work back. This is useful especially when the modified content has conflicts with other branches.

<!-- Create, switch, delete a branch -->

<!-- Git stash -->

<!-- Merge and resolve conflicts -->

<!-- Rebase and resolve conflicts -->

## GitHub and remotes

Remote repositories in Github

### From local to remote

Push code

### From remote to local

git clone

git fetch

git pull

### Pull requests

## References