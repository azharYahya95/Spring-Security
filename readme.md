## Spring Security

### Spring Security Custom Login Form
#### Step by Step Development Process
##### Step 1: Modify Spring Security Configuration

|Method|Description|
|---|---|
|configure(AuthenticationManagerBuilder)|Configure users (in memory, database, ldap, etc)|
|configure(HttpSecurity)|Configure security of web paths in application, login, logout etc|

__code example__
```java
@Configuration
@EnableWebSecurity
public class DemoSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth)throws Exception{
        // add our users for in memory authentication
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .formLogin()
            .loginPage("/showMyLoginPage")
            .loginProcessingUrl("/authenticateTheUser")
            .permitAll();
    }
}
```
__code explanation line by line__
|code|explanation|
|---|---|
|.authorizeRequests()|Restrict access based on the HttpServletRequest|
|.anyRequest.authenticated|Any Request to the app must be authenticated (ie logged in)|
|.formLogin("/showMyLoginPage")| We are customizing the form login process|
|.loginProcessingUrl("/authenticateTheUser")|Login form should POST data to this URL for processing (check user id and password)|
|.permitAll()| ALlow everone to see the login page. No need to be logged in|

##### Step 2: Develop a Controller to show the custom login form
```java
@Controller
public class LoginController {

    @GetMapping("/showMyLoginPage")
    public String showMyLoginPage(){
        return "plain-login";
    }
}
```

##### Step 3: Create custom login form
- Best practice is to use Spring MVC Form tag <form:form>
    - provides automatic support for security defenses
- Spring Security defines default names for login form fields
    - user name field: username
    - Password field: password
```jsp
<%@ taglib prefix="form"  uri="http://www.springframework.org/tags/form"%>
...
<form:form action"${pageContext.request.contextPath}/authenticateTheUser" method="POST">

    <p>
        User name: <input type="text" name="username" />
    </p>
    <p>
        Password: <input type="password" name="password" />
    </p>
    <input type="submit" value="Login">
</form:form>
```
- Why use Context Path?
    - allows us to dynamically reference context path of application
    - Helps to keep link relative to application context path
    - If you change context path of app, then links will still work
    - Much better than hard-coding context path ...


#### Login error message
##### Development Process
1. Modify custom login form
    1. Check the error parameter
    2. If error parameter exists, show an error message

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core">
...
<form:form action=".." method="..">
    <c:if test="${param.errpr != null}">
        <i>Sorry! Ypu entered invalid username/password.</i>
    </c:if>
```