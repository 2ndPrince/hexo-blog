---
title: Slack-Notification
catalog: true
date: 2020-11-08 03:24:24
subtitle:
header-img:
tags:
- Notification
- Slack
categories:
- DevOps
---
# Configure Slack Notifications from Github and Jenkins
---
## Github
1. Create a slack app for Github
https://COMPANY.slack.com/apps/A0F7YS2SX-github-enterprise-server

![TheApp](slack-enterprise.png)

2. Configure your app

3. Copy the webhook available
(The generic webhook generated from Slack is different from the configured webhook from the app here)

4. Put the configured webhook in Github->Settings->Hooks->Webhooks->Add Webhook
Add the copied webhook url into `Payload URL`
Select content type `application/json`
Select the events for notifications

## Jenkins
(Assume your enterprise Jenkins has slack-plugins setup)
1. Provide the channel name on `slackSend` command in our groovy script.
2. Make sure you belong to the right organization, then providing the slack channel name will be sufficient.
(DevOps team has configured plugins for us, so that we don't need to provide webhook url but only the channel name)

The only change you need to do is
```
stage("Slack Notification"){
   def status = succeeded ? "Success" : "Failure"
   slackSend(channel:"channel-name", message "*{status}:* ...
