## Spring Security

### BASIC Security (Users, apssword and roles)
#### Step By Step Development Process
##### Step 1: Create Spring Security Initializer
```java
import org.springframework.security.web.context.AbstractSecurityWebApplicationInitializer;

public class SecurityWebApplicationInitializer extends AbstractSecurityWebApplicationInitializer {

}
```

##### Step 2: Create Spring Security Configuration
```java

@Configuration
@EnableWebSecurity
public class DemoSecurityConfig extends WebSecurityConfigurerAdapter {

}
```

##### Step 3: Add users, passwords and roles
```java
@Configuration
@EnableWebSecurity
public class DemoSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception{
        // add our users for in memory authentication

        UserBuilder users = User.withDefaultPasswordEncoder();

        auth.withUser()users.username("john".password("test123").roles("EMPLOYEE"));
        auth.withUser()users.username("mary".password("test123").roles("MANAGER"));
        auth.withUser()users.username("susan".password("test123").roles("ADMIN"));
    }
}
```


