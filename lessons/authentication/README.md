[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly)

# Authenticating Users

Adding login and registration functionality to our Spring app.

#### Learning Objectives

- Using bCrypt to encrypt passwords upon registration and compare passwords during login.
- Using session to store the logged in user.
- Using react to allow login and registration of users and track the posts of each user.

#### Prerequisites

- Use the Spring Boot project we've built so far with CORS requests enabled and full CRUD for users and posts. 

---

When we want to add more functionality to our Spring apps, what's your first guess of what to look for? 

Hopefully you thought to add a dependency of some kind, perhaps by browsing the MvnRepository. If it's a very common pattern or need for web apps, you might also think of adding another Spring starter package. Add the following dependency in `pom.xml` to allow use of security features such as bCrypt. 

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
```

## Encrypting with bCrypt

Our first step will be to encrypt passwords when a user is created, thereby storing secure hashes instead of plain-text passwords. 

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

@Service("userService")
public class UserService {

    private UserRepository userRepository;
    private BCryptPasswordEncoder bCryptPasswordEncoder;

    @Autowired
    public UserService(UserRepository userRepository,
                       BCryptPasswordEncoder bCryptPasswordEncoder) {
        this.userRepository = userRepository;
        this.bCryptPasswordEncoder = bCryptPasswordEncoder;
    }

    public User findUserByUsername(String username) {
        return userRepository.findByUsername(username);
    }

    public User saveUser(User user) {
        user.setPassword(bCryptPasswordEncoder.encode(user.getPassword()));
        userRepository.save(user);
        return user;
    }

}
```

If the Service wants to use a userRepository method to find a user by username, we'd better make sure that function is defined! Add the following to the `userRepository.java`:

```java
User findByUsername(String username);
```
Now, if we tried running this project, we'd get an error telling us the bean bCryptPasswordEncoder can't be found:
`Consider defining a bean of type 'org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder' in your configuration.`

Very well! To make some packages effectively available to our application, we have to define a configuration file that will declare the BCryptPasswordEncoader as a bean (think of it as a module) available to our application. Create a new class named WebMvcConfig and copy the following:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        BCryptPasswordEncoder bCryptPasswordEncoder = new BCryptPasswordEncoder();
        return bCryptPasswordEncoder;
    }
}
```

This pattern lets us inject the password encoder throughout our application. Now that we're using the security package, we have to configure the security settings of our app with a new class called SecurityConfiguration:  

```java

@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {


    @Autowired
    private BCryptPasswordEncoder bCryptPasswordEncoder;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.cors().and().authorizeRequests()
                .antMatchers( HttpMethod.OPTIONS, "/**").permitAll()
                .antMatchers("/**").permitAll()
                .antMatchers("/login").permitAll().anyRequest()
                .authenticated().and().csrf().disable();
    }

    @Override
    public void configure(WebSecurity web) throws Exception {
        web
                .ignoring()
                .antMatchers("/resources/**", "/static/**", "/css/**", "/js/**", "/images/**");
    }
}
```

Now, we'll update our user creation route to make use of the new UserService method. Be sure to inject the UserService into the controller, the same way that we put the userRepository and postRepository in there (including the @AutoWired annotation).

In the controller:
```java
   @PostMapping("/users")
    public User createUser(@ModelAttribute User user){
        User createdUser = userService.saveUser(user);
        return createdUser;
    }
```

## Make a Login Route

Add to controller, while making the necessary imports (intelliJ should help you with that!)

```java
    //at the top of the class definition near userRepository and userService:
    private BCryptPasswordEncoder bCryptPasswordEncoder;
    // as another route listed after class variables
    @PostMapping("/login")
    public User login(@RequestBody User login, HttpSession session) throws IOException{
        bCryptPasswordEncoder = new BCryptPasswordEncoder();
        User user = userRepository.findByUsername(login.getUsername());
        if(user ==  null){
            throw new IOException("Invalid Credentials");
        }
        boolean valid = bCryptPasswordEncoder.matches(login.getPassword(), user.getPassword());
        if(valid){
            session.setAttribute("username", user.getUsername());
            return user;
        }else{
            throw new IOException("Invalid Credentials");
        }
    }
```

This way, the server will respond with a 500 status code if the login fails, and the user's information with status code 200 if the login succeeds. From the react front-end, you'll be able to parse the response and perform different actions based on the status code of the response.

We can also use `session.getAttribute()` to pull information out of session whenever needed, but be warned- the return type will often be too vague for what you need, so you may have to find a way to change the session data into the type you need.

### Try it out!

Add login/registration to the react app, being sure to parse the responses and store the logged in user in state upon successul login. Then, when that user creates a post, be sure the post is created with the logged-in user set as that post's creator. 


### Stretch Goals

- Only authorize the post's creator to do things like edit a post or delete a post.
- Use more of Spring Boot's built-in authentication methods and protocols. This will involve adding more to the Server Configuration.
- Add social media authentication.

---

*Copyright 2018, General Assembly Space. Licensed under [CC-BY-NC-SA, 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)*