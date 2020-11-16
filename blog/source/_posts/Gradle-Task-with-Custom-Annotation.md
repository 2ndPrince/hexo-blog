---
title: Gradle-Task-with-Custom-Annotation
catalog: true
date: 2020-11-15 14:12:08
subtitle:
header-img:
tags:
- Gradle
- Annotation
categories:
- From-Previous-Blog-General
---

# Gradle Task with Custome Java Annotation

## 1. Overview

As a spring developer, you often see these annotations.

``` lang=java
@SpringBootApplication
@Override
@Controller @RestController @Service @Configuration @Getter
```

Annotations work like a blackbox that you do not know what's inside, but it works somehow.
Some logic must be added after you compile. To know more about that and annotation's history, refer to [THIS](https://en.wikipedia.org/wiki/Java_annotation#History) document

---

## 2. What is Annotation

Line 6 on the left screen, you see `@SpringBootApplication` annotation.

![SpringBootApplication](1.png)

The right side screen shows its annotation info. You can command+click the annotation to view this detail.

Where do you tag an annotation?
We've seen it under class or field.

Let's look at this enum class `java.lang.annotation.ElementType` 

``` lang=java
public enum ElementType:
	
    /** Class, interface (including annotation type), or enum declaration */
    TYPE,
    
    /** Field declaration (includes enum constants) */
    FIELD,
    METHOD,
    PARAMETER,
    CONSTRUCTOR,
    LOCAL_VARIABLE,
    ANNOTATION_TYPE,
    PACKAGE,
    TYPE_PARAMETER,
    TYPE_USE,
    MODULE
```

Until when this annotation takes into effect?

Looking at `java.lang.annotation.RetentionPolicy`, there are three policies

``` lang=java
public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     *
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}
```

**It's a good habit to command+click on annotations that you want to know more.**

You don't see any logic under an annotation, but `annotationProcessor` creates its logic behind the scene. 

1. RetentionPolicy.SOURCE: Annotation whose content is ignored by compiler
2. RetentionPolicy.CLASS: Annotation whose content is recorded in the class file, but no need in runTime.
3. RetentionPolicy.RUNTIME: Annotation whose content is recoreded in the class file and also in runTime.

## 3. Create our annotation

Annotation is Interface because it doesn't have a concrete body.
Don't forget to put @ sign in front of interface.
Let's make three annotations for test (even, odd, prime) in `test package`

``` lang=java
package me.ndPrince.routeOptimization.annotation;

import org.junit.jupiter.api.Tag;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Tag("odd")
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface odd {
}
```

The tag `@Tag("odd")` is a new annotation introduced by junit junipter and helps us maintain project.
Create two other annotations `even` and `prime` in the similar manner.

Make the odd annotation's target as `TYPE` and even as `FIELD`

![Target](2.png)

Note that line 7 and 10 has RED lines underneath. Compile error because `ElementType` doesn't match.

Revert the target as `TYPE` for class, and observe the red line gone.

## 4. Test

Make five unit tests `TestOne` through `TestFive` and put its annotations accordingly (even annotation to even numbers, prime annotation to prime numbers)

``` lang=java
package me.ndPrince.routeOptimization;

import me.ndPrince.routeOptimization.annotation.even;
import me.ndPrince.routeOptimization.annotation.prime;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.test.context.junit.jupiter.SpringExtension;

@ExtendWith(SpringExtension.class)
@even @prime
class TestTwo {

    @Test
    void testTwo(){
        System.out.println("testing two");
    }
}
```

Number 2 is an even number and also of prime. (Two annotations)
Number 3 is an odd number. (One annotation)

Add the following `Gradle Task` in build.gradle file. (Refer to the previous post for gradle task tutorial)

![build-gradle](3.png)

Currently, gradle can't recognize `useJUnitPlatform`

Add the following.

``` lang=java
test {
	useJUnitPlatform {
		includeTags('even', 'odd', 'prime')
	}
}
```

According to [the Gradle-Java-Testing document](https://docs.gradle.org/current/userguide/java_testing.html), you can use Tags with JUnitPlatform.
You can specify what tags to include or exclude.

Make a change on this gradle task.

``` lang=java
task oddTest(type: Test){
	group "demo test"
	useJUnitPlatform{
		includeTags 'odd'
	}
}
```

We use `type` that refers to method in `useJUnitPlatform`

![Tags](4.png)

Look at the right side gradle window. Find the `demo test` group and three tasks underneath.
- `evenTest` executed one test; Four; I've used excludeTags to exclude Two
- `oddTest` executed three tests; One, Five, Three
- `primeTest` executed three tests; Five, Three, Two

Source code in [HERE](https://github.com/2ndPrince/routeOptimization/tree/spring-101-article-13)

---

## 5. Troubleshoot

1. The following error message appeared after adding Jupiter api dependency

![troubleshoot](5.png)

The reason is it was `main` where annotation is sitting, but the jupiter dependency uses `testImplementation`
Our custom annotations are only for tests, so move annotations in `test` directory instead of `main`.

2. `test events were not received` shown with no tests executed

Gradle thinks no code changes since before, and it didn't bother to run the test.
Try `./gradlew clean` and run the test.

## 6. Use Case

1. Jenkins pipeline can have one or more custom annotations
2. For front end tests (selenium), you can put different annotations for different tests(smoke, a/b, regression..)
