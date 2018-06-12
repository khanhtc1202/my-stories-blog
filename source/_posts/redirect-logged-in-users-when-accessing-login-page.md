---
title: Redirect logged-in users when accessing register page with Spring Security
date: 2018-06-12 23:36:36
tags: technical
thumbnail: https://i.imgur.com/De2PZ5B.png
---

# Introduction 

**TLDR** _How to redirect logged-in users when they access some specific pages such as: login, register,... with Spring Security._

Any website implements authentication need to solve a common problem, it is redirecting user when they access based on authentication (or authorization). For example: user will be redirect to login page when the user is not yet logged in; or user will be redirected to deny page when the user don't have enough permission.

Most of tutorial for implementing login/logout mechanism with Spring Boot in github guides using HttpSecurity class for route protection. But they do not account of not allowing user accessing login/register page when they already logged in; in my opinion it should be redirect to home page when they access /login page.  

After searching but not finding any solutions; I propose a method of using Principal and Authentication class of Spring Security.
 
# Content

A naive solution can be overriding the configure method in WebSecurityConfigurerAdapter class like this

```
@Override
  protected void configure(HttpSecurity http) throws Exception {
      http
          .authorizeRequests()
            // Something here :3
              .antMatchers("/register").anonymous()
              .antMatchers("/admin").hasRole("ADMIN")
              .and()
          .formLogin()
              .loginPage("/login")
              .usernameParameter("email")
              .passwordParameter("password")
              .defaultSuccessUrl("/user/profile")
              .failureUrl("/login?error")
              .and()
          .exceptionHandling()
              .accessDeniedPage("/");
  }
```

It will return the content of homepage (at /) if user try to register when already logged-in. But the problem is the url is still at /register. I tried to looking the correct solution by searching, reading document of the SpringSecurity; but I found no answers. 

*So i try a different way, by handling it in the Controller*

With @Controller annotated bean, the principal option can be defined directly as a method argument and it will be resolved by the framework. If user not yet logged in, its value will be null.

```java
@Controller
public class MainController {
    
      @GetMapping("/register")
      public String register(Model model,Principal principal) {
        if(principal!=null){
          return "redirect:/";
        }
        else{
          UserRegistrationDTO userRegistrationDTO = new UserRegistrationDTO();
          model.addAttribute(userRegistrationDTO);
    
          return "userregistration";
        }
      }
    
}
```

An another approach is using the Authentication class, just define in the method argument and the framework will auto injection it for you.
```java
@Controller
public class MainController {
    
      @GetMapping("/register")
      public String register(Model model, Authentication authentication) {
        if(authentication!=null){
          return "redirect:/";
        }
        else{
          UserRegistrationDTO userRegistrationDTO = new UserRegistrationDTO();
          model.addAttribute(userRegistrationDTO);
    
          return "userregistration";
        }
      }
    
}
```

You can also get more information of currently logged in user by using method Authentication.getAuthorities() or Authentication.getDetails()

That's all, please give a comment if you know a better solution.

# Reference material

http://www.baeldung.com/get-user-in-spring-security