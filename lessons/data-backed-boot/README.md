[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Connecting to a Database in Spring Boot

Using MySQL to add a database to a Spring Boot Application

#### Learning Objectives

- Configuring our application to connect with MySql
- Using MySQL workbench to create new databases and tables
- Setting up a model and repository to use the database

#### Prerequisites

- A basic spring boot application
- [Installing MySql and MySql Workbench](./Installing-MySql-Workbench.md)

---

## Getting Started

1. Follow the instructions in the [previous lesson](../starting-a-boot-project) to set up a basic Spring Boot web application.

1. Inside of pom.xml, we'll add some new dependencies to allow us to connect with a MySql database. Add the following to the list of dependencies in your pom.xml file:
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!-- JPA Data (We are going to use Repositories, Entities, Hibernate, etc...) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!-- Use MySQL Connector-J -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- This dependency fixed an error. Thanks, stack overflow! -->
        <dependency>
            <groupId>javax.xml.bind</groupId>
            <artifactId>jaxb-api</artifactId>
        </dependency>
```
1. Before we try to connect to a database with our app, we have to create the database first. Create a schema using MySql Workbench (or regular SQL from your command line using homebrew-installed mysql)

1. We will also need to set up some configuration for jdbc (java database connector) to know where our database lives and what credentials it needs to use. Inside the src/main/resources folder, create a file called `application.properties` and add the following, making sure to change YOUR_DATABASE_NAME to the actual name of the schema you created in MySql Workbench for this project.
```java
spring.jpa.hibernate.dll-auto=create
spring.datasource.url= jdbc:mysql://localhost:3306/YOUR_DATABASE_NAME
spring.datasource.username=root
spring.datasource.password=root

# ==============================================================
# = Show or not log for each sql query
# ==============================================================
spring.jpa.show-sql = true

# ==============================================================
# = Hibernate ddl auto (create, create-drop, update)
# ==============================================================
spring.jpa.hibernate.ddl-auto = update

# ==============================================================
# = The SQL dialect makes Hibernate generate better SQL for the chosen database
# ==============================================================
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect
```

1. Now let's make our Model and Repository for a Post. 

### Try it out!

Using Postman to test your routes, try setting up the full suite of 7 RESTful routes for a specific resource. Respond only with strings at this point that should send messages back confirming success of the request. You will have to look up how to use route parameters in a spring boot application!


### Resources

- [Getting started with spring boot](https://spring.io/guides/gs/spring-boot/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*