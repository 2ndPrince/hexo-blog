---
title: Create-Github-Repo
catalog: true
date: 2020-11-14 21:10:48
subtitle:
header-img:
tags:
- Github
categories:
- From-Previous-Blog-General
---

# Create Github Repository

## 1. Overview

Before VCS(Version Control Software), people named files like filename_v1 and filename_v2.
With VCS, you could track all history and easily work
with other people. In this post, we are going to create a private Github repository.

---

## 2. Create a private Repository

There are two types of repositories in github; public and private.
If you choose private and would like to invite other people, you might end up creating a github team account or enterprise that are not free.
We are working alone for this tutorial, and private is okay for us.

![Github-Private-Repo](1-private.png)

``` lang=bash
git init
git config --global user.email "git이메일주소"
git config --global user.name "git이름(별명)"
git remote add origin "깃주소"
(git remote set-url origin new_url) to edit
git add .
git commit -m "Initial commmit"
git push origin master
```

![Need-To-Create-SSH](2-push-origin-master.png)

Looking at the message, I was not permitted. 
Need to verify my credentials through password or SSH key.

## 3. Setup SSH Key

You could verify yourself with password, but you don't want to repeat yourself every time for a push.
You can save your password somewhere, but it can be exposed to others.
A better verification method is using SSH key.
This key is generated in your local machine and link it to your github account.
You don't save your password anywhere but this key only in your machine.
For different devices, you might end up creating new SSH keys, but not a big deal.

Follow up with [the guides](developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys) on setting up SSH Key.

## 4. Push your code

Try the push command once again. 
Your two commits have been pushed 4 minutes ago.

``` lang=bash
git push origin master
```

![Pushed](3-push-available.png)

---

## 5. Troubleshoot

When trying to push, if you get rejected for having the current branch's version lower than the previous one, and if you are sure that your branch is the latest one, you have an option to rebase.

``` lang=bash
git pull --rebase origin master
```
