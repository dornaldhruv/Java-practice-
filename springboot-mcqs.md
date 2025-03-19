# Spring Boot Online Assessment Questions

## Section 1: Spring Core Annotations

### Question 1
Which annotation is used to define a Spring Bean without specifying a custom name?

A) `@Bean`
B) `@Component`
C) `@Repository`
D) `@Configuration`

**Answer: B**

**Explanation:**
- `@Component` is a generic stereotype annotation used to indicate that a class is a Spring component. When used without specifying a name, the bean name defaults to the class name with the first letter in lowercase.
- `@Bean` is used at the method level to define a bean, not at the class level.
- `@Repository` is a specialized form of `@Component` specifically for persistence-related components.
- `@Configuration` indicates that a class declares one or more `@Bean` methods and may be processed by Spring to generate bean definitions.

### Question 2
Consider the following code:

```java
@Component
public class ServiceA {
    // Implementation
}

@Component
public class ServiceB {
    // Implementation
}

@Component
public class ServiceC {
    private final ServiceA serviceA;
    private final ServiceB serviceB;
    
    @Autowired
    public ServiceC(ServiceA serviceA, ServiceB serviceB) {
        this.serviceA = serviceA;
        this.serviceB = serviceB;
    }
}
```

Where is `@Autowired` annotation optional in this code?

A) It's mandatory and cannot be removed
B) It can be removed if there are setter methods instead
C) It can be removed since Spring 4.3 as constructor injection doesn't require it
D) It can only be removed if both dependencies are marked as `@Qualifier`

**Answer: C**

**Explanation:**
- Since Spring Framework 4.3, the `@Autowired` annotation is no longer necessary on constructors of a component class if the class has only one constructor.
- In this case, Spring automatically injects the dependencies serviceA and serviceB through the constructor without requiring the explicit `@Autowired` annotation.
- The rule only applies to single-constructor scenarios. If multiple constructors exist, `@Autowired` is still required to identify which constructor Spring should use for dependency injection.
- This is part of Spring's evolution toward more implicit and less verbose configuration.

### Question 3
Which of the following annotations creates a bean that will be initialized eagerly instead of lazily?

A) `@Bean(eager=true)`
B) `@Component(lazy=false)`
C) `@Bean` without any attributes
D) `@Scope("singleton")`

**Answer: C**

**Explanation:**
- By default, all Spring beans are initialized eagerly (at application startup) if they are singleton-scoped, which is the default scope.
- `@Bean` without any attributes creates a singleton-scoped bean that is initialized at application startup.
- There is no `eager=true` attribute for `@Bean`.
- There is no `lazy=false` attribute for `@Component`. Instead, `@Lazy(false)` would be used, but beans are eager by default anyway.
- `@Scope("singleton")` just explicitly declares the default scope, which is already eager by default.

### Question 4
What is the correct way to specify the order of initialization for Spring beans?

A) `@DependsOn("beanName")`
B) `@Order(1)`
C) `@Priority(100)`
D) Both B and C can be used for this purpose

**Answer: A**

**Explanation:**
- `@DependsOn` explicitly declares that the annotated bean should be initialized after the specified beans.
- `@Order` and `@Priority` are used for ordering beans of the same type in collection injections, but they don't directly control bean initialization order.
- `@Order` is primarily used for ordering components in arrays or lists when multiple beans of the same type exist.
- `@Priority` is similar to `@Order` but is from the `javax.annotation` package and has some different semantics.

## Section 2: Spring Bean Lifecycle and Configuration

### Question 5
What will happen if multiple `@Configuration` classes define beans with the same name?

A) The application will fail to start with a BeanDefinitionOverrideException
B) The bean defined last during scanning will override earlier definitions
C) The application will choose one bean randomly at runtime
D) The application will use the bean with the highest `@Order` value

**Answer: B**

**Explanation:**
- By default, Spring Boot allows overriding of bean definitions. When multiple beans with the same name are found, the last one defined during scanning wins.
- However, this behavior can be changed by setting `spring.main.allow-bean-definition-overriding=false` in application.properties, which would then cause the application to fail with a BeanDefinitionOverrideException.
- Starting from Spring Boot 2.1, the default for this property was changed to `false`, making option A correct for newer versions, but the traditional Spring behavior was option B.
- Bean overriding has nothing to do with `@Order` annotations.

### Question 6
Which lifecycle method will be called first when a bean is created?

A) `@PostConstruct` annotated method
B) `afterPropertiesSet()` from InitializingBean interface
C) Custom init method specified in `@Bean(initMethod="customInit")`
D) All of these happen in the order listed

**Answer: D**

**Explanation:**
- Spring bean initialization follows a specific order:
  1. First, methods annotated with `@PostConstruct` are called
  2. Next, the `afterPropertiesSet()` method is called if the bean implements InitializingBean
  3. Finally, any custom init method specified in the `@Bean` annotation is called
- This order allows for proper layering of initialization code, with standard annotations being processed first, followed by Spring-specific interfaces, and ending with custom methods.
- This sequencing gives developers flexibility in how they implement initialization logic.

### Question 7
When will a bean annotated with `@Lazy` be initialized?

A) At application startup
B) When first referenced by another component
C) When explicitly requested using ApplicationContext.getBean()
D) Both B and C

**Answer: D**

**Explanation:**
- `@Lazy` means the bean won't be initialized at application startup, which is the default behavior for singleton beans.
- A lazy-initialized bean is instantiated only when it's first needed, which can happen either when:
  - It's first referenced by another component being initialized
  - It's explicitly requested using ApplicationContext.getBean() or similar methods
- Lazy initialization can improve startup time but might shift initialization issues to runtime.
- Note that if a lazy bean is a dependency of a non-lazy bean, it will still be initialized at startup when its consumer is created.

## Section 3: Spring Boot Auto-Configuration

### Question 8
What happens if both a custom bean and an auto-configured bean of the same type exist in a Spring Boot application?

A) The application will fail to start with a ConflictingBeanDefinitionException
B) The custom bean always takes precedence over the auto-configured bean
C) The auto-configured bean takes precedence as it's part of the framework
D) The bean with the `@Primary` annotation will be used, or the custom bean if neither has it

**Answer: B**

**Explanation:**
- Spring Boot follows a principle called "user beans take precedence." This means that if you define a custom bean, it automatically overrides any auto-configured bean of the same type.
- This allows developers to replace Spring Boot's auto-configured components with their own implementations without explicitly disabling the auto-configuration.
- This is a core design decision in Spring Boot that enables you to selectively override just what you need.
- The `@Primary` annotation would only matter if there were multiple custom beans of the same type, but a custom bean always overrides an auto-configured one regardless of `@Primary`.

### Question 9
Which annotation would you use to disable a specific auto-configuration class?

A) `@DisableAutoConfiguration`
B) `@SpringBootApplication(disableAutoConfiguration=true)`
C) `@EnableAutoConfiguration(exclude=SomeAutoConfiguration.class)`
D) `@ConditionalOnMissingBean(SomeAutoConfiguration.class)`

**Answer: C**

**Explanation:**
- The correct way to disable specific auto-configuration classes is by using the `exclude` attribute of `@EnableAutoConfiguration`, which is also available on `@SpringBootApplication` since it includes `@EnableAutoConfiguration`.
- The full form would be `@EnableAutoConfiguration(exclude={SomeAutoConfiguration.class})` or `@SpringBootApplication(exclude={SomeAutoConfiguration.class})`.
- There is no `@DisableAutoConfiguration` annotation in Spring Boot.
- `@ConditionalOnMissingBean` is used within auto-configuration classes to conditionally create beans, not to disable entire auto-configuration classes.

### Question 10
What is the purpose of Spring Boot's `spring.factories` file?

A) It registers Spring beans for the application
B) It defines default property values
C) It lists auto-configuration classes to be loaded by Spring Boot
D) It configures the Spring application context hierarchy

**Answer: C**

**Explanation:**
- The `spring.factories` file is a mechanism used by Spring Boot to discover and load components using the `SpringFactoriesLoader`.
- Most commonly, it's used to list auto-configuration classes under the key `org.springframework.boot.autoconfigure.EnableAutoConfiguration`.
- It's typically located in the `META-INF` directory of a JAR file.
- This file enables Spring Boot's "convention over configuration" principle by automatically loading configurations without explicit declarations in the application code.
- Spring Boot uses this mechanism for various extension points, not just auto-configuration.

## Section 4: Spring Data JPA

### Question 11
Consider the following Spring Data JPA repository method:
```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByLastNameAndFirstName(String lastName, String firstName);
}
```

What SQL query will be generated when this method is called?

A) `SELECT * FROM user WHERE last_name = ? AND first_name = ?`
B) `SELECT u FROM User u WHERE u.lastName = ?1 AND u.firstName = ?2`
C) `SELECT * FROM user WHERE last_name = ?1 OR first_name = ?2`
D) `SELECT * FROM user WHERE last_name = ? OR first_name = ?`

**Answer: A**

**Explanation:**
- Spring Data JPA parses the method name and generates the appropriate SQL query based on the naming convention.
- For method names starting with `findBy`, followed by property names connected with logical operators like `And` or `Or`, Spring Data JPA creates a query with the corresponding WHERE conditions.
- The `findByLastNameAndFirstName` will generate a SQL query with `WHERE last_name = ? AND first_name = ?`, assuming column names are snake_case versions of the entity property names.
- The query is parameterized with the values passed to the method in the same order they appear in the method name.

### Question 12
Which annotation is used to specify a custom query in a Spring Data repository?

A) `@CustomQuery`
B) `@NamedQuery`
C) `@Query`
D) `@Repository("query=...")`

**Answer: C**

**Explanation:**
- `@Query` is the annotation used in Spring Data repositories to specify custom queries, either in JPQL (Java Persistence Query Language) or native SQL.
- For example: `@Query("SELECT u FROM User u WHERE u.email = ?1")`
- `@NamedQuery` is a JPA annotation used at the entity level, not in repositories.
- There is no `@CustomQuery` annotation in Spring Data.
- `@Repository` is used to mark a repository interface/class but doesn't accept query parameters.

### Question 13
What is the difference between `findById` and `getById` in Spring Data JPA repositories?

A) `findById` returns an Optional while `getById` throws an exception if the entity is not found
B) `findById` executes a query immediately while `getById` returns a proxy and only executes the query when the entity is accessed
C) `findById` is for primary keys while `getById` can work with any field
D) `findById` is part of CrudRepository while `getById` is a custom method that needs to be defined

**Answer: B**

**Explanation:**
- `findById` executes a SELECT query immediately and returns an Optional that might be empty if the entity is not found.
- `getById` (or `getOne` in older versions) is a JPA-specific method that returns a reference proxy without hitting the database. The actual SELECT query is executed only when the entity's properties are accessed (lazy loading).
- Both methods work with the entity's ID/primary key.
- Both methods are provided by the framework (through JpaRepository), and don't need to be defined manually.
- Note: In Spring Data JPA 3.0+, `getById` has been deprecated in favor of `getReferenceById`.

### Question 14
What will happen when this code is executed?

```java
@Service
@Transactional
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public void updateUser(User user) {
        User existingUser = userRepository.findById(user.getId()).orElseThrow();
        existingUser.setName(user.getName());
        // No explicit save call
    }
}
```

A) The user will not be updated since there's no call to `userRepository.save()`
B) The user will be updated because the entity is managed within the transaction
C) A compilation error will occur
D) The method will throw an exception at runtime

**Answer: B**

**Explanation:**
- When an entity is loaded within a transaction (via `findById` in this case), it becomes managed by the persistence context.
- Changes to a managed entity are automatically detected and synchronized with the database when the transaction commits.
- This is known as "dirty checking" in JPA.
- Therefore, even without an explicit call to `save()`, the changes to `existingUser` will be persisted to the database.
- This is a key feature of JPA but can sometimes lead to unexpected behavior if developers are not aware of it.

## Section 5: Spring Boot Actuator

### Question 15
Which actuator endpoint provides information about application health?

A) `/actuator/info`
B) `/actuator/health`
C) `/actuator/status`
D) `/actuator/metrics`

**Answer: B**

**Explanation:**
- The `/actuator/health` endpoint provides basic health information about the application.
- It can be extended to include health indicators for various components like database connections, disk space, etc.
- The `/actuator/info` endpoint provides arbitrary application information.
- The `/actuator/metrics` endpoint shows metrics information for the current application.
- There is no standard `/actuator/status` endpoint in Spring Boot Actuator.

### Question 16
By default, which Spring Boot Actuator endpoints are exposed over HTTP?

A) All endpoints
B) Only `/health` and `/info`
C) Only `/health`
D) No endpoints are exposed by default

**Answer: B**

**Explanation:**
- In Spring Boot 2.x, by default, only the `/health` and `/info` endpoints are exposed over HTTP.
- This is a security measure to avoid exposing potentially sensitive information.
- You can expose all endpoints by setting `management.endpoints.web.exposure.include=*` in your application properties.
- Alternatively, you can expose specific endpoints by listing them: `management.endpoints.web.exposure.include=health,info,metrics`
- The health endpoint itself by default only shows the application status (UP/DOWN) but can be configured to show more details.

### Question 17
Which property would you use to change the base path of actuator endpoints from `/actuator` to `/management`?

A) `spring.actuator.base-path=/management`
B) `management.server.servlet.context-path=/management`
C) `management.endpoints.web.base-path=/management`
D) `management.context-path=/management`

**Answer: C**

**Explanation:**
- The correct property is `management.endpoints.web.base-path=/management`, which changes the base path for all actuator endpoints.
- After this change, endpoints would be accessible at paths like `/management/health` instead of `/actuator/health`.
- `management.server.servlet.context-path` is used when you want actuator endpoints on a different port with a specific context path.
- `management.context-path` was used in older versions of Spring Boot (1.x) but is now deprecated.
- There is no `spring.actuator.base-path` property.

## Section 6: Spring Security

### Question 18
In Spring Security, what is the default authentication mechanism in a Spring Boot application?

A) LDAP Authentication
B) OAuth2
C) Form-based Authentication
D) HTTP Basic Authentication

**Answer: D**

**Explanation:**
- When you add Spring Security to a Spring Boot application without additional configuration, it defaults to HTTP Basic Authentication.
- Basic Authentication sends credentials as a Base64-encoded header value.
- Spring Boot's auto-configuration sets up a very basic authentication with a generated password printed in the logs.
- Form-based authentication requires explicit configuration but is commonly used in web applications.
- LDAP and OAuth2 require specific dependencies and explicit configuration.

### Question 19
Consider the following Spring Security configuration:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .antMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            .and()
            .formLogin();
    }
}
```

Which statement is correct about this configuration?

A) All requests must be authenticated except those to `/public/**`
B) Only requests to `/admin/**` need authentication
C) The login form is available at `/login` by default
D) A and C are both correct

**Answer: D**

**Explanation:**
- The configuration specifies that URLs matching `/public/**` are accessible without authentication (`.permitAll()`).
- URLs matching `/admin/**` require the user to have the ADMIN role.
- All other requests (`.anyRequest()`) require the user to be authenticated.
- The `.formLogin()` method configures form-based authentication with default settings, including a login page at `/login`.
- Therefore, both statements A and C are correct.

### Question 20
What is the purpose of the `@PreAuthorize` annotation in Spring Security?

A) It prevents unauthorized access before controller methods are executed
B) It authenticates users before they can access the application
C) It checks authorization before a method is invoked
D) It must be used on all secured endpoints

**Answer: C**

**Explanation:**
- `@PreAuthorize` is part of Spring Security's method security feature.
- It evaluates a Spring Expression Language (SpEL) expression before invoking the method.
- If the expression evaluates to false, access is denied and the method is not executed.
- For example: `@PreAuthorize("hasRole('ADMIN')")` ensures only users with the ADMIN role can execute the method.
- To use `@PreAuthorize`, you need to enable method security with `@EnableGlobalMethodSecurity(prePostEnabled=true)`.

### Question 21
What will happen when this Spring Security configuration is used?

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();
    }
}
```

A) The application will be vulnerable to CSRF attacks
B) The application will not allow any state-changing operations (POST, PUT, DELETE)
C) Users will be required to include CSRF tokens in all forms
D) The application will generate validation errors for all form submissions

**Answer: A**

**Explanation:**
- CSRF (Cross-Site Request Forgery) protection is enabled by default in Spring Security.
- Using `.csrf().disable()` turns off CSRF protection, making the application potentially vulnerable to CSRF attacks.
- CSRF attacks occur when malicious websites trick users' browsers into making unwanted requests to sites where they're authenticated.
- When CSRF protection is enabled, Spring Security requires a CSRF token in forms for state-changing HTTP methods (POST, PUT, DELETE).
- Disabling CSRF protection is sometimes done in REST APIs that use token-based authentication (like JWT), as they're typically immune to CSRF.

## Section 7: Spring Boot Application Properties and Profiles

### Question 22
Which property file has the highest precedence in Spring Boot?

A) `application.properties` in the classpath
B) `application.properties` in the current directory
C) `application-{profile}.properties` in the classpath
D) Command line arguments

**Answer: D**

**Explanation:**
- Spring Boot uses a defined order of precedence for property sources.
- Command line arguments have the highest precedence.
- Next come properties from `SPRING_APPLICATION_JSON`.
- Then properties from profile-specific files outside the packaged jar.
- Then properties from profile-specific files inside the packaged jar.
- Then application properties outside the packaged jar.
- Finally, application properties inside the packaged jar.
- This order allows for easy property overriding in different environments.

### Question 23
What happens when you define a property in both `application.properties` and `application-dev.properties`, and you start the application with the `dev` profile active?

A) The value from `application-dev.properties` takes precedence
B) The value from `application.properties` takes precedence
C) Spring Boot throws a ConfigurationConflictException
D) The application fails to start with a property conflict error

**Answer: A**

**Explanation:**
- When multiple property sources define the same property, the source with higher precedence wins.
- Profile-specific properties (`application-{profile}.properties`) have higher precedence than standard properties (`application.properties`).
- Therefore, properties defined in `application-dev.properties` override those in `application.properties` when the `dev` profile is active.
- This allows for environment-specific configuration overrides, which is a common use case for Spring profiles.
- No exceptions are thrown for property conflicts; the higher precedence value is simply used.

### Question 24
How do you activate multiple Spring profiles simultaneously?

A) By setting `spring.profiles.active=profile1;profile2`
B) By setting `spring.profiles.active=profile1,profile2`
C) By setting `spring.profiles.active=profile1` and `spring.profiles.include=profile2`
D) Both B and C are correct

**Answer: D**

**Explanation:**
- There are two ways to activate multiple profiles:
  1. By providing a comma-separated list in `spring.profiles.active`: `spring.profiles.active=profile1,profile2`
  2. By using `spring.profiles.include` to add additional profiles: `spring.profiles.active=profile1` and `spring.profiles.include=profile2`
- Profiles can be activated via properties files, environment variables, command-line arguments, or programmatically.
- The order of profiles matters as properties from later profiles override properties from earlier ones.
- Since Spring Boot 2.4, the `spring.config.activate.on-profile` approach is preferred within configuration files.

## Section 8: Spring Boot Testing

### Question 25
What is the purpose of the `@SpringBootTest` annotation?

A) It creates a lightweight mock context for testing repositories only
B) It loads a full application context for integration testing
C) It tests only the web layer without starting the server
D) It configures JUnit to work with Spring Boot

**Answer: B**

**Explanation:**
- `@SpringBootTest` loads a complete application context, similar to what would be created when running the application normally.
- It is used for integration testing where you need the full Spring context with all beans.
- This annotation is relatively heavyweight as it initializes the entire application.
- For more focused tests with partial context loading, other annotations like `@WebMvcTest`, `@DataJpaTest`, or `@JsonTest` are preferred.
- `@SpringBootTest` can be configured to start a real server, use a mock environment, or just load the ApplicationContext without a server.

### Question 26
Which annotation is used to mock a bean in the Spring context for testing?

A) `@InjectMocks`
B) `@MockBean`
C) `@Mock`
D) `@Spy`

**Answer: B**

**Explanation:**
- `@MockBean` is a Spring Boot annotation that adds a mock implementation of a bean to the Spring ApplicationContext.
- When used, any existing bean of the same type in the application context will be replaced with the mock.
- This is useful for isolating components for testing by replacing their dependencies with mocks.
- `@Mock` is a Mockito annotation that creates a mock but doesn't add it to the Spring context.
- `@InjectMocks` is a Mockito annotation that injects mocks into the tested object but doesn't interact with Spring.
- `@Spy` is for partial mocking where some real methods are called and others are stubbed.

### Question 27
What does the `@DataJpaTest` annotation do?

A) It loads the full application context for testing JPA repositories
B) It configures an in-memory database for testing without affecting the real database
C) It validates JPA queries at compile time
D) It creates mock repositories for all entities

**Answer: B**

**Explanation:**
- `@DataJpaTest` provides a convenient way to test JPA repositories by:
  - Configuring an in-memory database (like H2) by default
  - Setting up Hibernate, Spring Data, and the DataSource
  - Performing an `@EntityScan`
  - Enabling SQL logging
- It loads only the JPA components, not the entire application context, making tests faster.
- By default, it runs tests in a transaction that is rolled back after each test.
- This annotation doesn't validate JPA queries at compile time or create mock repositories automatically.

## Section 9: Advanced Spring Boot Concepts

### Question 28
What is the purpose of the `@Conditional` annotations in Spring Boot?

A) To conditionally enable security based on user roles
B) To conditionally run specific tests based on environment
C) To conditionally create beans based on various factors
D) To conditionally apply transaction management

**Answer: C**

**Explanation:**
- `@Conditional` annotations allow Spring Boot to conditionally create beans based on specific conditions.
- Common examples include:
  - `@ConditionalOnProperty`: Creates a bean only if a specific property has a specific value
  - `@ConditionalOnBean`: Creates a bean only if another specified bean exists
  - `@ConditionalOnMissingBean`: Creates a bean only if another specified bean doesn't exist
  - `@ConditionalOnClass`: Creates a bean only if a specified class is on the classpath
- These annotations are fundamental to Spring Boot's auto-configuration mechanism, allowing it to adapt to the application environment.
- They enable the "opinionated defaults with easy overriding" philosophy of Spring Boot.

### Question 29
Which statement about `@ConfigurationProperties` is correct?

A) It can only be used on `@Configuration` classes
B) It supports relaxed binding of external properties to Java beans
C) It is less type-safe than using `@Value` annotations
D) It doesn't support validation of properties

**Answer: B**

**Explanation:**
- `@ConfigurationProperties` supports relaxed binding, which means properties can be specified in different formats (camelCase, kebab-case, etc.) and will be correctly bound to Java bean properties.
- It can be used on any Spring bean, not just `@Configuration` classes, especially when combined with `@ConfigurationPropertiesScan` or `@EnableConfigurationProperties`.
- It is more type-safe than `@Value` because it binds entire hierarchical structures and supports the use of `javax.validation` constraints.
- It does support validation when combined with `@Validated`.
- Example: `@ConfigurationProperties(prefix = "app.mail")` would bind all properties starting with `app.mail` to the bean properties.

### Question 30
What is the purpose of Spring Boot's `CommandLineRunner` interface?

A) To execute code after the Spring ApplicationContext has been initialized
B) To parse command-line arguments passed to the application
C) To enable command-line interactions with a running application
D) To run the application in a command-line mode without web server

**Answer: A**

**Explanation:**
- `CommandLineRunner` is a functional interface that runs code after the Spring ApplicationContext has been initialized but before the application starts fully.
- It's commonly used for initialization tasks, data setup, or validation that needs to happen at application startup.
- The interface defines a single `run` method that receives command-line arguments as parameters.
- Multiple `CommandLineRunner` beans can exist in an application and their execution order can be controlled with `@Order`.
- Spring Boot also offers the similar `ApplicationRunner` interface which provides parsed arguments instead of raw strings.

## Section 10: Spring Dependency Injection and Bean Scopes

### Question 31
What will happen in the following scenario?

```java
@Configuration
public class AppConfig {
    
    @Bean
    public ServiceA serviceA() {
        return new ServiceA(serviceB());
    }
    
    @Bean
    public ServiceB serviceB() {
        return new ServiceB();
    }
}
```

A) Each time serviceA calls serviceB, a new instance of ServiceB will be created
B) A single instance of ServiceB will be shared by all calls from ServiceA
C) The application will fail to start due to a circular dependency
D) The serviceA bean will not be able to access serviceB

**Answer: B**

**Explanation:**
- When a `@Bean` method calls another `@Bean` method directly in a `@Configuration` class, Spring intercepts the call and returns the existing bean instance from the container.
- This is because Spring subclasses `@Configuration` classes using CGLIB to override the methods and provide bean instance caching.
- Therefore, even though the code appears to create a new `ServiceB` instance each time `serviceB()` is called, only one singleton instance is actually created.
- This behavior ensures proper singleton semantics even when beans reference each other directly.
- This would be different in a class annotated with `@Component` instead of `@Configuration`, where CGLIB proxying doesn't happen.

### Question 32
What is the difference between constructor injection and field injection in Spring?

A) Constructor injection ensures mandatory dependencies while field injection makes all dependencies optional
B) Constructor injection works with final fields while field injection doesn't
C) Field injection is preferred over constructor injection according to Spring best practices
D) Both A and B are correct

**Answer: D**

**Explanation:**
- Constructor injection has several advantages over field injection:
  1. It ensures that required dependencies are provided when the bean is constructed (no risk of NullPointerException from uninitialized dependencies).
  2. It allows for immutable dependencies by using final fields, which field injection cannot do.
  3. It makes testing easier as dependencies can be provided explicitly in tests without reflection.
- With field injection, dependencies are injected after object construction, so the object might temporarily be in an inconsistent state.
- Spring's own documentation recommends constructor injection as a best practice, contrary to option C.
- Therefore, both statements A and B are correct.

### Question 33
Which scope restricts a bean to the lifecycle of a single HTTP session?

A) request
B) session
C) application
D) prototype

**Answer: B**

**Explanation:**
- The `session` scope creates a single bean instance for the duration of an HTTP session.
- This is useful for beans that need to maintain state specific to a user's session.
- The `request` scope creates a new bean instance for each HTTP request.
- The `application` scope is essentially the same as singleton scope in a web application.
- The `prototype` scope creates a new instance each time the bean is requested from the container.
- Other scopes include `websocket` for the lifecycle of a WebSocket and custom scopes that can be defined.

## Section 11: Spring Boot Web and REST

### Question 34
What is the purpose of the `@RestController` annotation?

A) It combines `@Controller` and `@ResponseBody`
B) It enables Spring MVC request handling
C) It configures Spring Security for REST endpoints
D) It allows a controller to handle both REST and MVC requests

**Answer: A**

**Explanation:**
- `@RestController` is a convenience annotation that combines `@Controller` and `@ResponseBody`.
- `@Controller` marks the class as a web controller capable of handling requests.
- `@ResponseBody` indicates that method return values should be bound to the web response body, bypassing view resolution.
- By using `@RestController`, every method in the controller automatically returns data (like JSON) directly to the HTTP response body.
- This annotation is specifically designed for building RESTful web services where the return data isn't rendered in a view.

### Question 35
What is the HTTP status code returned by default when a controller method throws an `EntityNotFoundException`?

A) 404 (Not Found)
B) 400 (Bad Request)
C) 500 (Internal Server Error)
D) 204 (No Content)

**Answer: C**

**Explanation:**
- By default, any uncaught exception thrown from a controller method will result in a 500 (Internal Server Error) status code.
- Spring Boot doesn't have special handling for `EntityNotFoundException` out of the box.
- To make `EntityNotFoundException` return a 404 status, you would need to configure exception handling, typically using `@ExceptionHandler` or `@ControllerAdvice`.
- Exception handling is a critical aspect of building robust REST APIs, allowing you to convert specific exceptions to appropriate HTTP status codes.

### Question 36
What is the purpose of Spring Boot's `@RequestParam` annotation?

A) To bind a request parameter to a method parameter
B) To validate query parameters against a predefined format
C) To include request parameters in the response