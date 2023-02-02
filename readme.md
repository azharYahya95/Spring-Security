## Spring Security

### Getting Started
#### Overview
##### 1. Document Referal
http://www.luv2code.com/spring-security-reference-manual

##### 2. Spring Security with Sevlet Filters
- Spring Filters are used to pre-process / post-process web requests
- servlet Filrers can route web requests based on security logic
- Sping provides a bulk of security functionality with servlet filters

##### 3. Declarative Security
- Define applications security constraints in configuration
    - All Java config (@Configuration, no xml)
    - or Spring XML config
- Provides separation of concerns between application code and security

##### 4. Programatic Security
- Spring security provides an API for custome application coding 
- Provides greater customization for specific app requirements

#### Spring Configuration
##### 1. Development Process
- Add Maven dependencies for Spring MVC Web App
    - list of api that want to add in pom.xml
        - spring-webmvc
        - jstl
        - javax.servlet-api
        - jstl
        - javax.servlet.jsp-api
    - customize Maven build in pom.xml
        - Need to customize Maven build. Since we are not using web.xml
        - must add maven war plugin
    ```
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <!-- Add Maven coordinates (GAV) for: maven-war-plugin -->
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-war-plugin</artifactId>
                    <version>3.2.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
    ```
- Create Spring App Configuration (@Configuration)
    - Enabling the MVC Java Config
        ```
            @EnableWebMVC
        ```
        - Provides similar support to <mvc:annotation-driven/> in XML
        - Adds conversion, formatting and validation support
        - Processing of @Controller classes and @RequestMapping etc.. methods
    - code example
        ```java
            @Configuration
            @EnableWebMVC
            @ComponentScan(basePackages="com.luv2code.springsecurity.demo")
            public class DemoAppConfig{
                //define a bean for ViewResolver

                @Bean
                public ViewResolver viewResolver(){
                    InternalResourceViewResolver viewResolver = 
                            new InternalResourceViewResolver();
                    viewResolver.setPrefix("/WEB-INF/view/");
                    viewResolver.setSuffix(".jsp");

                    return viewResolver;
                }
            }
        ```
- Create Spring Dispatcher Servlet Initializer
    - Web Initializer
        - Spring MVC provides support for web app initialization
        - Makes sure your code is automatically detected
        - your code is used to initialized the servlet container
        <br>`AbstractAnnotationConfigDispatcherServletInitializer`
        - todo list
            - Extend this abstract base class
            - Override require methods
            - Specify servlet mapping and location of your app config
    ```java
    public class MySpringMVCDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

        @Override
        protected Class<?>[] getRootConfigClasses() {
            // todo generated method stub
            return null;
        }

        @Override
        protected Class<?>[] getServletConfigClasses(){
            return new Class[] {DemoAppConfig.class};
        }

        @Override
        protected String[] getServletMappings() {
            return new String[] {"/"};
        }
    }
    ```
- Develop our Spring controller
```java
    @Controller
    public class DemoController {

        @GetMapping("/")
        public String showHome(){
            return "home";
        }
    }
```
- Develop our JSP view page

##### To check on compatible version of Spring Security and Spring Framework
https://www.udemy.com/course/spring-hibernate-tutorial/learn/lecture/9193928#notes

