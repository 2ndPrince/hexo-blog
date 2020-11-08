---
title: spring-initializr
catalog: true
date: 2020-11-05 03:21:14
subtitle:
header-img:
tags:
categories:
---

# Start a project with Spring Initializr

## Overview
The journey of creating a software involves many tasks such as dependency management, testing, packaging and deployment. Build automation tools such as Maven and Gradle can reduce your burden on these time-consuming tasks.

In this post, let’s create a spring-boot project with auto-generated dependencies by spring initializr along with our convenient tool, Gradle. Then, we will create an REST endpoint in localhost with a health actuator endpoint.

## Create a Spring-boot Project
Visit Spring Initialzr, a website tool that generates spring-boot project for you.
123
- Gradle Project
- Java 8
- Spring Boot version
- Fill in project metadata
- Add dependencies; Web and Actuator

![LiveMyLife Desktop](web-ui.png)

## Create a demo controller

Create an endpoint “/demo/message” that takes in a string and returns the length of your input string.1

{% codeblock lang:java %}
@RestController
@RequestMapping("/demo")
public class ScheduleController {

    @GetMapping(value = "/message/{text}")
    public String returnLength(@PathVariable String text){
		return text.length();
	}
}
{% endcodeblock %}

## Test
Launch application and visit localhost:8080/demo/message from your internet browser, equivalent of GET request from postman.

---

## Troubleshooting
To validate your spring dependency imports, go to External Libraries in Intellij.
For example, an annotation ‘@SpringBootApplication’ is in the import path ‘org.springframework.boot:spring-boot-autoconfigure’. If this annotation is unrecognized and shows in red, it is likely that your Gradle failed to import this library.