---
title: Create-Endpoint-using-RestController
catalog: true
date: 2020-11-12 22:48:06
subtitle:
header-img:
tags:
categories:
---


# Create an Endpoint using RestController

## Overview
Last time, using Spring-Initializr, we created a springboot project.
In this post, we are going to create a simple rest endpoint and test it on browser.

## Create a Project
Use Spring-Initializr.
Add dependencies; Web, Actuator along with Java 8

## Create a demo controller
If you added the `actuator` dependency, you already have one endpoint on your application. The path is `localhost:8080/actuator/health` in case your app port is 8080. Let's create an endpoint of our own.

{% codeblock lang:java %}
@RestController
@RequestMapping("/demo")
public class DemoController {

    @Value("${message:default message}")
    private String message;

    @GetMapping("/message")
    public String getMessage(){
        return message;
    }
}
{% endcodeblock %}

application.properties
```
message=hi
```

## Test
Launch the application and go to `localhost:8080/demo/message` on browser.
The browser shows the GET response.

## Troubleshooting
You can check what spring dependencies are imported using `External Libraries` in IntelliJ. For example, the annotation `@SpringBootApplication` is inside the library `org.springframework.boot:spring-boot-autoconfigure`.
If this annotation is marked red and not recognized, the chance is Gradle's import failure on this library.