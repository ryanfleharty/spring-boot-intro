[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Supporting CORS Requests from React

Enable CORS requests to allow a React front-end to interact with our Java Spring server.

#### Learning Objectives

- Configure CORS requests to our server
- Create a full stack Java-React Application 

#### Prerequisites

- A basic spring boot application as as described in the [first](../starting-a-boot-project) [three](../data-backed-boot) [lessons](../relationships-between-models)

---

Create a new Java class called CorsConfig. Add the following:

```java
package hello;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {

        registry.addMapping("/**")
                .allowedOrigins(
                        "http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "HEAD", "OPTIONS")
                .allowCredentials(true)
                .allowedHeaders("*");
    }
}
```

Now you can make requests from a react application! 

### Try it out!

Add a react front-end that allows you to create posts and users. Use a drop down menu when creating posts to allow the choice of which user the post should belong to. Have the associated posts shown in the user's Show page, and list the post's creator when showing posts in the index or show routes.

### Resources

- [Getting started with spring boot](https://spring.io/guides/gs/spring-boot/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*