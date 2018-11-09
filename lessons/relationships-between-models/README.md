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
    private User user;
```

The @ManyToOne annotation says that this field of the post model will create a relationship with another table, where Post is the "many" side and the User is the "one" side (i.e. one user has many posts. Many posts can be joined to One user.)

The @JoinColumn specifices which column in the post table will hold the foreign key- the id that links to a different table. 

This will also change the way we create a post, considering that the user's id won't be in the POST data sent in the request. We won't be implementing authentication quite yet, but we should set up our createPost function to allow for a user to be connected to each post. Use the UserRepository to query for a specific user (could be any existing user- we're just hardcoding the creator of each post for now) and attach them to each post you create.

Now when we query for a post, it will have the user attached. But what about the other way around? You might have guessed we can add a similar annotation to the User model:

```java
    @OneToMany(cascade = CascadeType.ALL, mappedBy="user")
    private Set<Post> posts;
```

Here, `mappedBy` is referring to what property of the Post model refers to the specific user.  Cascading is a database term meaning that the related model will be affected by changes to the current model, i.e. if a user is deleted, so are their posts. 

### Try it out!

Use Postman to test full CRUDability of users and posts, making sure that when a user is deleted, so are their associated posts. Also, try adding another model that would establish one to many relationships, such as comments, and/or another model that would establish a many to many relationship, such as "liking" a post. 


### Resources

- [Getting started with spring boot](https://spring.io/guides/gs/spring-boot/)

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*