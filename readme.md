## Spring Security

### Spring Adding Logout Support
#### Step By Step Development Process
##### Step 1: Add logout support to Spring Security Configuration
add:
```
.and()
.logout().permitAll();
```

##### Step 2: Add logout Button
- Send Data to default logout URL: /logout
    - By Default, must use POST method
```html
<form:form action="${pageContext.request.contextPath}/logout" method="POST">
    <input type="submit" value="Logout">
</form:form>
```

##### Step 3: Update login form to display "logged out" message
1. Update login form
    - Check the logout parameter
    - If logout parameter exists, show "logged out" message
```jsp
<%@ taglib prefix="c" uri="htt[://java.sun.com/jsp/jstl/core"  %>
...
<form:form action"..." method="...">
    <c:if test="${param.logout != null}">
        <i>You have been logged out</i>
    </c:if>

    User name: <input type="text" name="username" />
    Password: <input type="password" name="password" />

</form:form>
...
```
