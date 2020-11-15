---
title: Link-Cloud-Database-With-PCF
catalog: true
date: 2020-11-14 13:47:57
subtitle:
header-img:
tags:
- PCF
categories:
- From-Previous-Blog-General
---


# Link Cloud Database with PCF applications

## 1. Overview

Every application needs database. With our PCF cloud applications, we would need cloud database as well.
In this post, we are going to use `ClearDB MySQL database`, conveniently configured by PCF, link it with our application and finally test in order to confirm working.

---

## 2. Create a database in PCF

Among the list of services in PCF dashboard, choose this service.

![DB Service](db-instance.png)

## 3. Member credentials

Look into the database service created. Find the three information ready.

Hostname, Username and Password

## 4. Create database schema

With the help of UI from `DataGrip`, you can easily write SQL script(DDL) to create and modify table structure.

Create Table
![DB Table Creation](new-table-datagrip.png)

View Table with data
![DB Table Creation](db-table-with-data.png)

View SQL Queries
![DB Query](db-queries-generated.png)

## 5. Get a database value through demo application

Let's think about how to do it before jumping into the implementation.

- First, the application should know database connection info.
- Second, the application should know the table and column names.
- Third, the application should have a model class to contain the database read values.
- Fourth, the application should have an endpoint to make a call to the database and return the result to us.

Let's create the model class called `Employee` that contains id and name.
![Employee model class](employee-pojo.png)

Let's create a repository that makes call from application to database. 

Use spring data jpa and it will work with other database types(Oracle, MySql... etc).

![Employee Repository](employee-repo.png)

Let's create a rest controller endpoint for us to test the connection.
![Employee Controller](employee-endpoint.png)

## 6. Test the connection from local machine

``` lang=yaml
spring.datasource.url=jdbc:mysql://us-cdbr-east-02.cleardb.com:3306/ad_c91762cf7ca78d6
spring.datasource.username=
spring.datasource.password=
```

Test form Postman
![Postman Test](test-postman.png)

## 7. Test the connection from PCF

Since we've just created the `getAllEmployees` endpoint, we need to push our application to the PCF in order to reflect the change.

``` lang=bash
cf login -a api.run.pivotal.io
cf push -f pathToManifest/manifest.yml
```

Test from PCF
![PCF Test](cloud-test.png)

---

## 8. Troubleshoot

1. DDL should be carefully designed beforehand, or you will end up changing multiple times

2. I've bound the database service to the application in PCF. However, I still provide the database connection info. There must be a room to improve

3. Curious to know generated queries? Set `logging.level.root=debug` in application.properties. Call the endpoint and find the queries in Intellij console. You will see `hql, hibernate, jpatransactionmanager` and `JpaTransactionManager, EntityManagerFactory, PoolBase`. Database connection details are taken care by `com.mysql.cj.jdbc.ConnectionImpl`.

4. The max user is four in our database. If the following error occurs, connect to the cleardb and clear existing connections.

``` lang=html
java.sql.SQLSyntaxErrorException: User 'XXXXX has exceeded the 'max_user_connections' resource (current value: 4)
```
