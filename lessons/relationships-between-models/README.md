[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Establishing Relationships between Models

Create an app with two inter-related models. 

#### Learning Objectives

- Create a Spring Boot app with two models
- Establish a one to many relationship
- Return query results 

#### Prerequisites

- A basic spring boot application as as described in the [first](../starting-a-boot-project) [two](../data-backed-boot) lessons

---

Add a private user field of type User to your Post model, which will represent the foreign key pointing to the post's creator. Be sure to add a getter and setter for the user field as well. Add the following annotations to that field:

```java
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
```

The @ManyToOne annotation says that this field of the post model will create a relationship with another table, where Post is the "many" side and the User is the "one" side (i.e. one user has many posts. Many posts can be joined to One user.)

The @JoinColumn specifices which column in the post table will hold the foreign key- the id that links to a different table. 

This will also change the way we create a post, considering that the user's id won't be in the POST data sent in the request. We won't be implementing authentication quite yet, but we should set up our createPost function to allow for a user to be connected to each post. Use the UserRepository to query for a specific user (could be any existing user- we're just hardcoding the creator of each post for now) and attach them to each post you create.



### Try it out!

Try adding full CRUDability for two models and include the related "one" model when querying for the many side, i.e. a Post also has the user's data attached.


### Resources

- [Getting started with spring boot](https://spring.io/guides/gs/spring-boot/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*