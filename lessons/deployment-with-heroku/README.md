[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Deploying with Heroku

Deploy your Java Spring Boot server on Heroku to power fully accessible applications on the internet.

#### Learning Objectives

- Deploy the Java server for your full-stack application on Heroku

#### Prerequisites

- A completed Java Spring Boot application
- Installed Heroku CLI tools

---

Commit your project to a local git repository to prepare for deployment to Heroku. At the root level of your project (where you can see `pom.xml`) run the following: 

`git init`
`git add .`
`git commit -m "ready for deployment"`

Then, create a new heroku application and connect it with the local repository. This can be done by either running a `heroku create` command directly in the terminal, or by using the heroku.com interface and pasting the relevant commands to add a `heroku remote` connection to your local repository.

When you execute `git push heroku master`, the application should build successfully, but throw errors upon attempting to connect to a SQL database.

#### Adding and Connecting to MySQL

We will use the `ClearDB` add-on to attach a simple SQL server to our Heroku application. You can either go to the `resources` section of your Heroku App's dashboard online, or run the command:

`heroku addons:create cleardb:ignite`

This will automatically create a `CLEARDB_DATABASE_URL` config var, which you can view from the dashboard or with the command `heroku config`. This generated url contains all the information you will need to connect to the remote SQL server directly from MySQL workbench!

The url contains the following information:

`mysql://USERNAME:PASSWORD@URL-TO-CONNECT-TO/DEFAULT-SCHEMA?reconnect=true`.

In MySql Workbench, you can create a new connection by providing the details from this url in the necessary fields. The SQL server will be running on port 3306. Once you've established the connection, you can perform all the same Table creation commands we used to set up our database locally. This should get rid of the deployed application's connection errors!

Now you can make requests to your server from a deployed react application or Postman! You will likely have to change the specifics in your CorsConfig to point to the address of your deployed Heroku app, in addition to changing the React app's fetch calls to now point to the deployed Java application. 

### Try it out!

Test your routes in Postman by making requests to your deployed application's url. You should see the expected JSON responses from your application if the deployment was successful.

### Resources

- [Heroku Documentation](https://devcenter.heroku.com/articles/deploying-spring-boot-apps-to-heroku)
- [ClearDB Heroku Add-On](https://elements.heroku.com/addons/cleardb)
---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*