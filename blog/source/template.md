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

## 2. AAAAAAAAAA

We already have one application and need one more for this exercise.
Let's use Spring Initializr just like before.
Only web dependency has been added.
The applicatio name is `demo-application` and ported at `8000`
In local environment, your host is `localhost` and need to distinguish apps by port number.
But in cloud, your main app and child app would have different host name, so no port number is necessary.

![RestController](1-RestController.png)

On the new application, create the following RestController and make sure the app running on port `8000`.

## 3. AAAAAAAAAA

On your main application, create the following GET endpoint that calls demo application's `/demo/message` endpoint using `RestTemplate`.

- Create a new instance of RestTemplate. `RestTemplate restTemplate = new RestTemplate();`
- Provide the two info on `getForEntity` method; Call URL and Response Class Type

![RestTemplate](2-RestTemplate.png)

## 4. AAAAAAAAAA

Launch both `main` and `demo` applications. Hit `localhost:8080/demo/call`.

![Result](2-RestTemplate-Result.png)

RestTemplate makes a synchronous call. 
Refer to `org.springframework.web.client.RestTemplate` and its javadoc for additional infomation.

---

## 5. Troubleshoot

- Add dependency `org.Springframework.http`
- `postForEntity` won't work. Your endpoint should be for `GET`. The log message says we've got post request but the app can't support.
![Not-Supported](3-Troubleshoot1.png)

- Difference between `postForObject` and `postForEntity` is as follows

``` lang=html
Compared to postForObject(), postForEntity() returns the response
as a ResponseEntity object. Other than that, both methods do the same job.
new HttpEntity<String>(personJsonObject. toString(), headers);
```
