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

Follow the instructions in the [previous lesson](../starting-a-boot-project) to set up a basic Spring Boot web application.

Inside of pom.xml, we'll add some new dependencies to allow us to connect with a MySql database. Add the following to the list of dependencies in your pom.xml file:
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- These two dependencies fix java version errors for newer Java -->
        <dependency>
            <groupId>javax.xml.bind</groupId>
            <artifactId>jaxb-api</artifactId>
        </dependency>
        <dependency>
            <groupId>org.javassist</groupId>
            <artifactId>javassist</artifactId>
            <version>3.23.1-GA</version>
        </dependency>
```
Before we try to connect to a database with our app, we have to create the database first. Create a schema using MySql Workbench (or regular SQL from your command line using homebrew-installed mysql)

We will also need to set up some configuration for jdbc (java database connector) to know where our database lives and what credentials it needs to use. Inside the src/main/resources folder, create a file called `application.properties` and add the following, making sure to change YOUR_DATABASE_NAME to the actual name of the schema you created in MySql Workbench for this project.
```java
spring.jpa.hibernate.dll-auto=create
spring.datasource.url= jdbc:mysql://localhost:3306/YOUR_DATABASE_NAME
spring.datasource.username=root
spring.datasource.password=WHATEVER_PASSWORD_YOU_SET_DURING_INSTALL

```

Now let's make our Model and Repository for a Post. Java Spring Boot applications typically use the pattern of a class-defined Entity and an automatically configured "Repository" to connect with a database table.

Let's start with a model. Create a new class in your package and name it `Post.java`. You can define your Post class as follows:

```java
import javax.persistence.*;

@Entity
public class Post {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String text;

    public String getText() {
        return text;
    }

    public void setText(String textInput){
        this.text = textInput;
    }

    public Long getId(){
        return id;
    }

    public void setId(Long id){
        this.id = id;
    }
}
```

Now we need a Repository to set up queries based on this model. Let's create a PostRepository file inside the same package we've been using. Fortunately, Spring Boot has made this file extremely easy to set up:

```java
// This will be AUTO IMPLEMENTED by Spring into a Bean called postRepository
// CRUD refers Create, Read, Update, Delete

public interface PostRepository extends CrudRepository<Post, Integer> {

}
```

That's it! Now we need to make some slight modifications to our Controller to allow it to use this repository to make queries. Add the following snippet inside the class definition for your controller.

```java
    @Autowired
    private PostRepository postRepository;
```

Now, you can set up a route that will fetch all of the posts and return them.

```java
    @GetMapping("/posts")
    public Iterable<Post> getPosts(){
        return postRepository.findAll();
    }
```

POST requests can be mapped to the attributes of the model you're trying to create. @ModelAttribute can be used inside the parameters of a controller function to define the post data according to a model.

```java
    @PostMapping("/posts")
    public Post createPost(@ModelAttribute Post post){
        Post createdPost = postRepository.save(post);
        return createdPost;
    }
```

### Try it out!

Finish the other five RESTful routes for posts and verify functionality using Postman to test each route. Then, try it again with a User model with usernames and passwords- avoid copy-pasting the Post Controller to attempt to recall as much as you can of how to set up a controller, model, and repository. 

You will have to account for some difficulties along the way, such as dealing with the strange data type returned by the repository's FindById method, and how to accept route parameters. You may also want to figure out how to handle the submission of invalid data from the form.


### Stretch Goals

- Add [project lombok](https://projectlombok.org/) to refactor and simplify your Models
- Try connecting to MongoDB instead of MySql
- Speed up common requests with [caching](https://spring.io/guides/gs/caching/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*