## Spring Security

### Cross Site Request Forgery (CSRF)
#### What is CSRF?
A security attack where an evil website tricks you into executing an action on a web application that you are currently logged in

#### CSRF Examples
1. You are logged into your banking app
    - tricked into sending money to another person
2. You are logged into an e-commerce app
    - tricked into purchasing unwanted items

#### CSRF Protection
- to protect against CSRF attacks
- Embed additional authentication data/token into all HTML forms
- On subsequent requests, web app will verify token before processing

#### When to use CSRF Protection?
1. The Spring Security team recommends to use CSRF protection for any normal browser web requests
2. If you are building a service for non-browser clients
    - you may want to disable CSRF protection (after careful review)

#### Best Practice of using Spring Security CSRF protection
- For form submissions use POST instead of GET
- Include CSRF token in form submission
- <form:form> automatically adds CSRF token
- If you don't use <form:form>, you must manually add CSRF token

#### Manually add CSRF token
```html
<form action="..." method="POST">
        <input type="hidden"
            name="${_csrf.parameterName}"
            value="${_csrf.token}">
</form>
```

#### CSRF Resources
1. CSRF Security Reference
https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)

2. Spring Security CSRF Support
https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#csrf
