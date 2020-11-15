---
title: Launch-Application-On-PCF
catalog: true
date: 2020-11-13 23:05:50
subtitle:
header-img:
tags:
- PCF
categories:
- From-Previous-Blog-General
---


# Launch an Application on PCF

# 1. Overview
Your localhost application can't be easily accessed by other people.
Your computer is not a server computer that provides stable responses through large number of requests. Your personal computer has a security issue when it's used as a web server. Server monitoring would be also of a burden.

Companies use cloud services to host their applications on the web. In this article, we are going to explore Pivotal Cloud Foundary(PCF) to host our localhost application. Additional features such as logging, monitoring, alerting and security can be easily integrated inside this cloud platform as plugins.

---

# 2. Create your application's JAR file
Execute `bootJar` option in Gradle. Check the JAR generated in `build/libs` path

# 3. Create an manifest file
What is a manifest? It's a text file that contains deploy information about the JAR file.
After you push your JAR file on the cloud, the manifest file guides the platform how to use.
For example, the manifest file tells the cloud system to allocate 1GB of memory with ABC logging service and BCD authentication service.

![Manifest](manifest.png)

# 4. Login to PCF
Open terminal and enter the following `cf login -a api.run.pivotal.io`
You will be prompted to provide more login information

![PCF Login](pcf-login.png)

# 5. Push your app into PCF
Two things you need; JAR and manifest
Use the following command to push.

`cf push -f {pathToManifest}/manifest.yml`

![PCF Push](pcf-push.png)

The state of `running` confirms the app has been pushed and running.

# 6. Check PCF console
You can also check the PCF console or dashboard to check the status of your applications.
To view, visit the console website.
`https://console.run.pivotal.io/`

![PCF Console](pcf-console.png)

# 7. Make a test call on the pushed app from the cloud
Use the Route that's assigned to the app. Use the demo endpoint that's created in the previous post.
It is `https://routing-optimizer.cfapps.io/demo/message`
You see the message `hi`

---

# Troubleshoot
1. You get some free credits to explore PCF platform. Don't spend money
2. Make sure the jar path on the manifest file