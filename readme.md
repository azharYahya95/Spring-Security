## Spring Security

### User Roles
#### Step by Step Development Process
##### Step 1: Update POM file for Spring Security JSP Tag Library
```
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-taglibs</artifactId>
    <version>${springsecurity.version}</version>
</dependency>
```

##### Step 2: Add Spring Security JSP Tag Library to JSP page
```
<%@ taglib prefix="security" 
        uri="http://www.springframework.org/security/tags" %>
```

##### Step 3: Display User ID
```
<%@ taglib prefix="security" 
            uri="http://www.springframework.org/security/tags" %>
User: <security:authentication_property="principal.username"/>
```

##### Step 4: Display User Roles
```
<%@ taglib prefix="security" 
            uri="http://www.springframework.org/security/tags" %>
User: <security:authentication_property="principal.authorities"/>
```


### Restrict Access Based on Roles
#### Step by Step Development Process

##### Step 1: Create Supporting Controller code and View Pages
|path|role|
|---|---|
|/|EMPLOYEE|
|/leaders|MANAGER|
|/systems|ADMIN|

##### Step 2: Update Our Roles
|User ID| Password| Roles|
|---|---|---|
|john|test123|EMPLOYEE|
|mary|test123|EMPLOYEE, MANAGER|
|susan|test123|EMPLOYEE, ADMIN|

```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception{
    // add our users for in memory authentication

    UserBuilder users = User.withDefaultPasswordEncoder();

    auth.inMemoryAuthentication()
        .withUser(users.username("john").password("test123").roles("EMPLOYEE"))
        .withUser(users.username("mary").password("test123").roles("EMPLOYEE" , "MANAGER"))
        .withUser(users.username("susan").password("test123").roles("EMPLOYEE" , "ADMIN"));
}
```

##### Step 3: Restricting Access to Roles
- Update your Spring Security Java configuration file (.java)
- General Syntax
```
antMatchers(<<add path to match on>>).hasRole(<< authorized role>>)
```
- code example
```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
        .antMatchers("/").hasRole("EMPLOYEE")
        .antMatchers("/leaders/**").hasRole("MANAGER")
        .antMatchers("/systems/**").hasRole("ADMIN")
        .and()
        .formLogin()
        ...
}
```

#### Configure custom page access denied
```java
Override
protected void configure(HttpSecurity http) throws Exception{
    ...
    .exceptionHandling()
        .accessDeniedPage("/access-denied");
}
```

#### Display Content Based on roles
- use Spring Security JSP Tags
```html
...
<security:authorize access="hasRole('MANAGER')">
    ......
<security:authorize>
```



