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

Configuration and new project

### Single chain

Commits, logs and revert

### Branches

Create, switch, delete a branch

Git stash

Merge and resolve conflicts

Rebase and resolve conflicts

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