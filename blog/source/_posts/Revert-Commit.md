---
title: Revert-Commit
catalog: true
date: 2020-11-15 13:01:42
subtitle:
header-img:
tags:
- Github
categories:
- From-Previous-Blog-General
---

# Revert a specific git commit

## 1. Overview

While working as a team, you might want to exclude a specific commit that is accidently made by yourself or teammates.
It doesn't matter whether the branch has been merged already or not.
With this method, we are going to create a branch that only reverts a specific commit within a PR that contains multiple commits.

---

## 2. Make a branch and a commmit

``` lang=bash
git branch test-revert
git checkout test-revert
### Make some changes to be reverted ###
git add .
git commit -m "commit to be reverted"
git push
```

## 3. Make a PR and merge it

![Merged](1-merged.png)

Note that this PR has two commits. Let's merge this PR and pretend we did so by accident and want to revert it back.

## 4. Choose a commit to REVERT

![Revert](2-Revert.png)

There's a `Revert` button on bottom right. If you click this, you will create a commit that reverts all commits not a specific one.

We only want to revert a commit `fc259e6`. We will use `git reset` command to do so.

``` lang=bash
git revert fc259e6
```

## 5. Choose commit to RESET

With reset, you are going to the specific commit and forget all commits done after.
The commits done after can be totally lost(reset hard) or can be in your local changes(soft).

``` lang=bash
git reset --hard 3e55218863e71ea01816b00aca09aa0203fbf755
git push -f origin head
```

![Reset](3-Reset.png)

---

## 6. Conclusion

`Git Reset` and `Git Revert` are different.

`Reset` means you are taking a time machine to go back to the specific commit.
All commits after the specified commit will disappear

`Revert` means you are making a surgery to take out the specific commit.

In `Intellij`, you can use its UI to view commit history and make reset or revert as you wish.
