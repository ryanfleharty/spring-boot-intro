[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Starting a Spring Boot Application

Using Maven to build a Spring Boot project.

#### Learning Objectives

- Using Maven to package larger projects
- Setting up a basic controller
- Running and testing our Spring Boot app

#### Prerequisites

- Basic Java and XML
- Familiarity with Maven build tool

---

## Getting Started

In your intelliJ text editor, start a new project configured with Maven. Do not choose any archetypes to start from. You should see the familiar src/main/java folder structure from the lesson on building Maven projects.

Inside of pom.xml, we'll add the necessary dependency to get started with Spring Boot. Fortunately, the necessary packages to quickly get a project running have been bundled into `starters`. Add the following to your pom.xml file, directly underneath the `<version>` tags, still inside the `<project>` tags.

```xml
    <!-- > Define the project as using spring boot starters <-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
    </parent>
    <!-- > Add a starter to your list of dependencies. 
    This is where new packages will be added as we add functionality. <-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

Now that our dependencies are set up, let's create a new package inside the src/main/java folder. You could name this package however you'd like - I just called it `hello` for our classic hello world example.

Inside of this package/directory, create a new class called `Application` to serve as the entry point into our app. This class will contain our `main` function, which will actually run the application. 

``` java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args){
        System.out.println("Hello out there!");
        SpringApplication.run(Application.class, args);
    }
}
```

You should notice our first use of an Annotation above a class definition. Annotations add functionality to a class, and will be used frequently throughout Spring Boot applications. You can also test out intelliJ's auto-import feature by deleting the import statements, causing the annotation to turn red. You can right click and import or click it and hit option+enter to add the import statement.

Let's test drive the app now! From the root directory of the project in your command line, run the command `mvn package`. This will install the dependencies and bundle our app into a single executable `.jar` file. You should see Maven building and compiling our project, and hopefully see a BUILD SUCCESS message.

Now that Maven has packaged the project, you can run the `.jar` file that lives inside the `/target` directory. Run the command: `java -jar target/<project-name>-1.0-SNAPSHOT.jar`. Use autocomplete to help you with this command instead of typing out the whole long file name.

When you run the file, you should see a whole bunch of output in the terminal, and at the top should be your "hello world" message.

So what? We got a server to log a message, big deal. Let's add a RestController to allow us to respond to requests! Make another class named MyController in the same package/directory as your Application class, and add the following to it:

```java
@RestController
public class MyController {
    @GetMapping("/")
    public String hello(){
        return "hello out there";
    }
}
```

More annotations! This time, @RestController lets us configure a class to be a Spring Boot Controller, and @GetMapping lets us accept a GET request at a specific route and respond by executing our function. You will need to auto-import these annotations before running the app! You can imagine there must also be @PostMapping to accept POST requests, and a more general @RequestMapping would let you accept requests of any method. 

Now that you've made a change to the app, re-package it with `mvn package` and run the file again. This time, if you visit localhost:8080 in your browser, you should see your message!

### Try it out!

Using Postman to test your routes, try setting up the full suite of 7 RESTful routes for a specific resource. Respond only with strings at this point that should send messages back confirming success of the request. You will have to look up how to use route parameters in a spring boot application!


### Resources

- [Getting started with spring boot](https://spring.io/guides/gs/spring-boot/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*