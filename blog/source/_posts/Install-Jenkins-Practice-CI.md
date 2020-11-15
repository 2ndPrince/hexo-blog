---
title: Install-Jenkins-Practice-CI
catalog: true
date: 2020-11-14 21:45:01
subtitle:
header-img:
tags:
- Jenkins
- DevOps
categories:
- From-Previous-Blog-General
---

# Install Jenkins Locally and Practice CI

## 1. Overview

Software engineer should also know CI/CD.

``` lang=html
CI/CD bridges the gaps between development and operation activities and
teams by enforcing automation in building, testing and deployment of applications.
```

With CI/CD properly configured, your applications will be automatically built, tested and deployed.

---

## 2. Download Jenkins Image through Docker

Install Docker in your local machine and pull jenkins image.
[Instruction](https://hub.docker.com/_/jenkins)
Follow the steps from the official instruction link above.

``` lang=bash
docker pull jenkins
```

Don't forget the initial password generated in the middle of instruction commands.

``` lang=bash
*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

7e17f066c15b48da9745dfcdf9115cc3

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```

## 3. Launch Jenkins

Using the Jenkins image, launch Jenkins on your local machine.

``` lang=bash
docker run -p 8080:8080 -p 50000:50000 jenkins
```

## 4. Configure Jenkins

Follow post-installation steps.
Visit `localhost:8080` and install suggested plugins.

![Local Jenkins](1-Local-Jenkins.png)

---

## 5. Troubleshoot

- Failed to install suggest plugins. Stackoverflow says try with the latest versions and it worked.

``` lang=bash
docker pull jenkins/jenkins:lts
docker run -p 8080:8080 jenkins/jenkins:lts
```
