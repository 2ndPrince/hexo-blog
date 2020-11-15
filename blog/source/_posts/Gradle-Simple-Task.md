---
title: Gradle-Simple-Task
catalog: true
date: 2020-11-15 14:00:17
subtitle:
header-img:
tags:
- Gradle
categories:
- From-Previous-Blog-General
---

# Gradle Simple Task

## 1. Overview

Spring developer would use either Maven or Gradle for dependency management tool.
A `gradle project` is made of several `gradle tasks`. A gradle task can be creating a JAR, generating Javadoc, or publishing some archives to a repository.
In this post, we are going to explore how to use gradle task.
This post is based on the official gradle tutorial found [here](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#tutorial_using_tasks)

---

## 2. Hello World

`build.gradle` is the core file. With this file, you can manage project depepdency and also can add tasks.

``` lang=html
Everything in Gradle sits on top of two basic concepts: projects and tasks.
```

Let's a simple task in this file.
``` lang=java
task hello {
    doLast {
        println 'Hello world!'
    }
}
```

Try `gradle -q hello` in windows
Try `gradlew -q hello` in macOS terminal

![Simple Task](1-Simple.png)

## 3. Some Coding with build script

``` lang=java
task upper {
    doLast {
        String someString = 'mY_nAmE'
        println "Original: $someString"
        println "Upper case: ${someString.toUpperCase()}"
    }
}
```

![Code](2-Code.png)

``` lang=java
task count {
    doLast {
        4.times { print "$it " }
    }
}
```

![Code2](3-Code.png)

## 4. Task dependencies

``` lang=java
task intro {
	dependsOn hello
	doLast {
		println "I'm Gradle"
	}
}
```

![Dependency](4-Dependency.png)

## 5. Extra Task Properties

The tutorial has more sections, but this is my last one. (Some of them is redundent)

``` lang=java
task myTask {
    ext.myProperty = "myValue"
}

task printTaskProperties {
    doLast {
        println myTask.myProperty
    }
}
```

![Final](5-Final.png)

---

## 6. Conclusion

In this post, we've learnt basic grammar of `Gradle Task`.
In the next post, we are going to create tasks that are relevant to real world application.
