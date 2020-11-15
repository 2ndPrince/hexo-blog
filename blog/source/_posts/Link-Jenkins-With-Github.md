---
title: Link-Jenkins-With-Github
catalog: true
date: 2020-11-14 22:34:01
subtitle:
header-img:
tags:
- Jenkins
- Github
- DevOps
categories:
- From-Previous-Blog-General
---

# Link Jenkins with Github

## 1. Overview

What does it mean to link Jenkins with Github?
Without this, you would have your source code, build, test, deploy the jar to others.
Using cloud, you would want to do differently.

When a PR has been merged, it triggers Jenkins build to build, test and deploy to a cloud platform such as PCF. In this post, we are going to have a Jenkins auto-triggered by a Github merge.

---

## 2. Create a Jenkins job

Choose `Freestyle-project`

![Freestyle](1-Freestyle.png)

This is a multi-purpose project that can be used to build project and easily integrated with git.

## 3. Configure the job

Enter your Github credentials in SCM.

![SCM](2-SCM.png)

Choose `GitHub hook trigger for GITScm polling` under `Build Triggers`

## 4. Build command

Add `Execute shell` under `Build`

``` lang=bash
./gradlew clean build -x test
```

The most explicit way to build is using execute shell.
You can try the same build command on your local machine and expect what to happen in advance. The command above means clean build without running tests. (Refer to Gradle for more info)

## 4. Try Job

Click `Build now` and check out the `Console Output`

![Console Output](3-Console.png)

The job has accessed our github repo's master branch and executed the gradle build command without any issues.

## 5. Setup a webhook in Github repo

Your github repo would trigger a Jenkins build by a webhook.

On the `Payload URL` you need to provide your Jenkins Job URL.
A problem is that your Jenkins is hosted by your local machine.
Just for practice purpose, we can use `ngrok`, a tool that creates a secure tunnel on your local machine along with a public URL.

Downolad ngrok from [here](https://dashboard.ngrok.com/get-started/setup)

``` lang=bash
unzip ngrok.zip 
./ngrok authtoken 1ekiMNhi4kcl3IqEQG1e89gQPqn_7bhs1mhkgCrecoLkLW2Us
Authtoken saved to configuration file: /Users/ylee55/.ngrok2/ngrok.yml
./ngrok http 8080
```

Try to visit the forwarding address in my case `https://a030dc835b77.ngrok.io`.
You will see your local Jenkins.
This public URL will access your local machine's port 8080 for this exercise.

![Ngrok](4-Ngrok.png)

Go back to the repository settings
![Webhook](5-Webhook.png)

Add `/github-webhook/` after your public Jenkins URL. Do not forget `/` at the end. It won't work if missing. If your `Recent Deliveries` has failures, resolve first before moving forward.

## 6. Test

Create a PR and make it merged.
Go to your jenkins and watch the build triggered.

![Create PR](6-Create-PR.png)

![Merge PR](6-Merge-PR.png)

![Auto trigerred](7-AutoTriggered.png)

From the image, you can confirm that the build has been `Started by Github push by 2ndPrince`

For now, it only executes clean and build, but you might want to add deploy option in the future.

---

## 7. Troubleshoot

- Make sure your Ngrok port is same as your localhost jenkins `8080`. If you follow tutorial, you may have it running at `80`
- You need to be an admin of your repository to see `Settings` menu on the repo
- Don't forget `/` at the end of `/github-webhook/`
