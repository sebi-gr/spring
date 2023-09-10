# 1	Container, Dependency, and IOC


1.1 What is dependency injection and what are the advantages?

Dependency injection is a design pattern in Spring that allows objects to be loosely coupled by providing their dependencies from external sources. Advantages include easier testing, flexibility in changing implementations, and better modularity.

1.2 What is an interface and what are the advantages of making use of them in Java?

An interface in Java defines a contract that classes must implement. Advantages include enabling polymorphism, separating abstraction from implementation, and facilitating multiple inheritance.

1.3 What is meant by “application-context?

An "application-context" in Spring is a container that manages Spring beans and their dependencies, providing configuration and services for an application.

1.4 How are you going to create a new instance of an ApplicationContext?

In Spring Boot, you create an ApplicationContext using the `SpringApplication.run()` method, like this:

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(MyApplication.class, args);
    }
}
```

This method is commonly used in Spring Boot applications to bootstrap and create the ApplicationContext.

1.5 Can you describe the lifecycle of a Spring Bean in an ApplicationContext?

The lifecycle of a Spring Bean in an ApplicationContext involves initialization (e.g., `@PostConstruct` methods) and destruction (e.g., `@PreDestroy` methods) phases. Beans are created when needed and destroyed when the ApplicationContext is closed.

1.6 How are you going to create an ApplicationContext in an integration test?

In an integration test, you can create an ApplicationContext using test-specific configurations, typically by using annotations like `@SpringBootTest` or `@ContextConfiguration`.

1.7 What is the preferred way to close an application context? Does Spring Boot do this for you?

The preferred way to close an application context is by calling `close()` on the context object. In Spring Boot, the context is often closed automatically when the application exits.

1.8 Can you describe Dependency injection using Java configuration?

Dependency injection using Java configuration involves creating a configuration class annotated with `@Configuration` and defining beans using `@Bean` methods. Example:

```java
@Configuration
public class MyConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```

1.9 Extra: What is the difference between explicit and implicit bean definition?

Explicit bean definition refers to defining beans programmatically, while implicit bean definition relies on conventions and annotations to discover and configure beans.

1.10 Can you describe Dependency injection using annotations (@Autowired)?

Dependency injection using annotations (@Autowired) injects dependencies automatically by inspecting the class's constructors, fields, or methods. Example:

```java
@Component
public class MyService {
    @Autowired
    private MyRepository repository;
}
```

1.11 Can you describe Component scanning and Stereotypes?

Component scanning is a process where Spring automatically discovers classes annotated with stereotypes like `@Component`, `@Service`, `@Repository`, and registers them as beans in the ApplicationContext.

1.12 Can you describe Scopes for Spring beans? What is the default scope?

Scopes in Spring define the lifecycle of a bean. The default scope is "singleton." Other scopes include "prototype," "request," "session," and more.

1.13 Are beans lazily or eagerly instantiated by default? How do you alter this behavior?

Beans are lazily instantiated by default, meaning they are created when first requested. To alter this behavior, you can use the `@Lazy` annotation.

1.14 What is a property source? How would you use @PropertySource?

A property source in Spring allows you to externalize configuration properties. `@PropertySource` is used to specify the location of property files.

1.15 What is a BeanFactoryPostProcessor and what is it used for? When is it invoked?

A BeanFactoryPostProcessor is used to customize the bean factory's configuration before bean instantiation. It's invoked during the bean factory's initialization.

1.16 What is a BeanPostProcessor and how is it different from a BeanFactoryPostProcessor? What do they do? When are they called?

A BeanPostProcessor is used to customize bean instances after their creation. It's different from BeanFactoryPostProcessor as it operates on individual beans and is invoked for each bean during initialization.

1.17 Init and destroy methods

Init and destroy methods in Spring beans can be annotated with `@PostConstruct` and `@PreDestroy`, respectively, to define custom initialization and destruction behavior.

1.18 Consider how you enable JSR-250 annotations like @PostConstruct and @PreDestroy? When/how will they get called?

To enable JSR-250 annotations like `@PostConstruct` and `@PreDestroy`, you need to configure a `CommonAnnotationBeanPostProcessor`. They are called automatically based on their respective lifecycles.

1.19 What does component-scanning do?

Component-scanning automatically detects and registers Spring beans based on component stereotypes, reducing the need for explicit bean definitions.

1.20 What is the behavior of the annotation @Autowired with regards to field injection, constructor injection, and method injection?

The `@Autowired` annotation can be used for field, constructor, or method injection. It automatically resolves and injects dependencies based on the injection point.

1.21 What do you have to do if you would like to inject something into a private field? How does this impact testing?

To inject something into a private field, you can use reflection or provide a public setter method to set the field's value. However, this approach can impact testing because private fields are not directly accessible, making it more challenging to mock or substitute dependencies during testing.

1.22 How does the @Qualifier annotation complement the use of @Autowired?

The `@Qualifier` annotation complements the use of `@Autowired` when you have multiple beans of the same type and need to specify which one to inject. By providing the bean's name as a qualifier, you indicate which specific bean should be injected.

1.23 What is a proxy object, and what are the two different types of proxies Spring can create?

A proxy object in Spring is a dynamically generated object that acts as an intermediary between a client and the target object. Spring can create two types of proxies:
   - JDK dynamic proxies, used when the target object implements interfaces.
   - CGLIB proxies, used when the target object doesn't implement any interfaces.

1.25 What does the @Bean annotation do?

The `@Bean` annotation is used to explicitly define a bean in a Spring configuration class. It tells Spring that a method annotated with `@Bean` will return an object that should be registered as a Spring bean in the application context.

1.26 What is the default bean id if you only use @Bean? How can you override this?

The default bean id when you use `@Bean` is the name of the `@Bean` method itself. You can override this by providing a custom name as an argument to the `@Bean` annotation, like this:

```java
@Bean("myCustomBeanName")
public MyBean myBean() {
    return new MyBean();
}
```

1.27 Why are you not allowed to annotate a final class with @Configuration?

You are not allowed to annotate a final class with `@Configuration` because Spring needs to create proxy classes for `@Configuration` classes to handle aspects like transaction management, and it can't do that for final classes.

1.28 How do @Configuration annotated classes support singleton beans?

Classes annotated with `@Configuration` support singleton beans because Spring treats them as sources of bean definitions. When you define a bean method in a `@Configuration` class, the resulting bean is a singleton by default within the application context.

1.29 Why can't @Bean methods be final either?

`@Bean` methods cannot be final because Spring needs to override these methods to handle bean creation and management. Making them final would prevent Spring from extending and customizing the behavior of these methods.

1.30 How do you configure profiles? What are possible use cases where they might be useful?

Profiles are configured using the `@Profile` annotation or the `spring.profiles.active` property in the `application.properties` or `application.yml` file. Profiles allow you to define different sets of beans and configurations for different environments (e.g., development, production). They are useful for managing configuration variations across different deployment scenarios.

1.31 Can you use @Bean together with @Profile?

Yes, you can use `@Bean` in conjunction with `@Profile` to conditionally define beans based on active profiles. Beans annotated with `@Profile` will only be created and registered if the specified profile(s) are active.

1.32 How to activate a profile?

You can activate a profile in Spring by setting the `spring.profiles.active` property in your application's configuration. For example, in `application.properties`:

```properties
spring.profiles.active=dev
```

Alternatively, you can set this property as a command-line argument or in your application's environment.

1.33 Can you use @Component together with @Profile?

Yes, you can use `@Component` in conjunction with `@Profile` to conditionally register a component bean based on active profiles. The component will only be instantiated and available in the application context if the specified profile(s) are active.

1.34 How many profiles can you have?

There is no strict limit to the number of profiles you can define in Spring. You can create as many profiles as needed to suit your application's specific requirements.

1.35 How can you get the current active and default profiles from Spring?

You can obtain the currently active profiles and the default profiles programmatically using the `Environment` object. For example, in a Spring bean:

```java
@Autowired
private Environment environment;

public void printActiveProfiles() {
    String[] activeProfiles = environment.getActiveProfiles();
    String[] defaultProfiles = environment.getDefaultProfiles();
    // Access the active and default profiles here.
}
```

1.36 How do you inject scalar/literal values into Spring beans?

You can inject scalar/literal values into Spring beans using the `@Value` annotation. This annotation allows you to inject values from property files, environment variables, or inline literals into fields or constructor parameters of your beans.

1.37 What is @Value used for?

The `@Value` annotation is used to inject values, such as property values or literals, into Spring bean fields or constructor parameters. It's a way to externalize configuration values and make them available to your beans.

1.38 What is Spring Expression Language (SpEL for short)?

Spring Expression Language (SpEL) is a powerful expression language used in Spring to query and manipulate objects at runtime. SpEL supports a wide range of expressions and can be used for tasks like conditional bean creation, property access, and more.

1.39 What is the Environment abstraction in Spring?

The Environment abstraction in Spring provides a way to access and manage properties and profiles within an application. It allows you to retrieve configuration values, check active profiles, and more, making your application more flexible and adaptable to different environments.

1.40 Where can properties in the environment come from – there are many sources for properties – check the documentation if not sure. Spring Boot adds even more.

Properties in the environment can come from various sources, including property files, system properties, environment variables, command-line arguments, and more. Spring Boot, in addition to these sources, provides its own set of conventions for configuring properties, making it even more flexible.

1.41 What can you reference using SpEL?

Using SpEL, you can reference and manipulate various objects and properties, including:
   - Bean properties and methods.
   - Environment properties and profiles.
   - System properties and environment variables.
   - Literal values and arithmetic expressions.
   - Collection and array elements.
   - And more.

1.42 What is the difference between $ and # in @Value expressions?

In `@Value` expressions:
   - `$` is used for accessing and resolving property values directly, such as `@Value("${my.property}")`.
   - `#` is used for evaluating Spring Expression Language (SpEL) expressions, which allows for more complex operations, like `@Value("#{myBean.calculateValue()}")`.

The choice between `$` and `#` depends on whether you're accessing simple property values or performing more advanced operations with SpEL.

# 2	Aspect Oriented Programming

2.1 What is the concept of AOP? Which problem does it solve? What is a cross-cutting concern?

AOP (Aspect-Oriented Programming) is a programming paradigm that addresses the separation of cross-cutting concerns from the main application logic. Cross-cutting concerns are aspects of a program that affect multiple modules or components, like logging, security, and transaction management. AOP solves the problem of code scattering and promotes cleaner, more maintainable code by allowing these concerns to be defined separately and applied where needed.

2.2 What is a pointcut, a join point, an advice, an aspect, weaving?

In AOP:
   - A **pointcut** specifies where in the code execution flow advice should be applied.
   - A **join point** is a specific point during program execution, such as method invocation or exception handling.
   - **Advice** is the code that runs at a particular join point; it represents the behavior you want to add to your application.
   - An **aspect** is a module that encapsulates related concerns (advices and pointcuts) into a single unit.
   - **Weaving** is the process of integrating aspects into the application code.

2.3 How does Spring solve (implement) a cross-cutting concern?

Spring implements cross-cutting concerns through Aspect-Oriented Programming (AOP). It allows you to define aspects that encapsulate behaviors such as logging, security, and transaction management separately from the main application logic. These aspects can be woven into the application code at specified join points using pointcut expressions.

2.4 Which are the limitations of the two proxy-types?

The two common types of proxies in Spring AOP have limitations:
   - **JDK dynamic proxies** can only proxy classes that implement interfaces, limiting their use to interface-based components.
   - **CGLIB proxies** can proxy classes without interfaces, but they cannot intercept `final` methods and classes or those marked as `private`, `static`, or `final`.

2.5 How many advice types does Spring support, and can you name each one?

Spring supports five types of advice:
   - **Before advice:** Runs before the advised method execution.
   - **After returning advice:** Runs after the advised method successfully returns a result.
   - **After throwing advice:** Runs after an advised method throws an exception.
   - **After (finally) advice:** Runs after the advised method, regardless of its outcome.
   - **Around advice:** Wraps the advised method, allowing custom behavior before and after its execution.

2.6 What do you have to do to enable the detection of the @Aspect annotation?

To enable the detection of the `@Aspect` annotation, you need to add `<aop:aspectj-autoproxy />` or `@EnableAspectJAutoProxy` to your Spring configuration. This tells Spring to scan for classes annotated with `@Aspect` and apply their aspects.

2.7 If shown pointcut expressions, would you understand them?

Yes, I can help you understand pointcut expressions. If you provide specific pointcut expressions, I can explain what join points they match and where advice will be applied in your code.

2.9 What is the JoinPoint argument used for?

The `JoinPoint` argument in AOP advice methods provides information about the currently executing join point, such as the method being invoked and the target object. It allows you to access details of the method call, such as method arguments and the target object, and provides context for performing custom behavior in advice.

2.10 What is a ProceedingJoinPoint? When is it used?

A `ProceedingJoinPoint` is a special type of `JoinPoint` used in around advice. It represents the join point at which the advice is being invoked and allows you to control the execution of the advised method. You can choose to proceed with the method's execution or modify its behavior by invoking `proceed()` on the `ProceedingJoinPoint`. It's typically used when you want to intercept and possibly modify the method's input, output, or execution flow.

# 3	Data Management: JDBC

3.1 What is the difference between checked and unchecked exceptions?

Checked exceptions must be explicitly declared in the method signature or handled using try-catch blocks, while unchecked exceptions (also known as runtime exceptions) do not have these requirements. Checked exceptions represent recoverable errors, while unchecked exceptions typically represent programming errors or unrecoverable situations.

3.2 Why does Spring prefer unchecked exceptions?

Spring prefers unchecked exceptions because they do not clutter method signatures with exception declarations or force developers to handle exceptions that are often unrecoverable. Unchecked exceptions are more convenient for handling exceptions that should typically result in the termination of the current operation or program.

3.3 What is the data access exception hierarchy?

The data access exception hierarchy in Spring includes `DataAccessException` as the root exception, which has several subtypes like `DuplicateKeyException`, `EmptyResultDataAccessException`, and `DataIntegrityViolationException`. These exceptions provide a structured way to handle data access-related issues in a Spring application.

3.4 How do you configure a DataSource in Spring? Which bean is very useful for development/test databases?

In Spring, you can configure a DataSource using properties in your application configuration file (e.g., `application.properties` or `application.yml`). The `HikariCP` DataSource bean is often recommended for development and test databases due to its excellent performance and reliability.

3.5 What is the Template design pattern, and what is the JDBC template?

The Template design pattern is a behavioral design pattern that defines the structure of an algorithm but allows subclasses to implement the steps. The JDBC template is a Spring class that follows the Template design pattern to simplify database access by providing common operations and handling low-level details like connection management and exception handling.

3.6 What is a callback? What are the three JdbcTemplate callback interfaces that can be used with queries? What is each used for?

A callback in the context of the JDBC template is a functional interface that allows you to customize the behavior of database operations. The three common callback interfaces for queries are:
   - `RowMapper`: Used to map rows from the result set to Java objects.
   - `ResultSetExtractor`: Used to extract data from the entire result set.
   - `RowCallbackHandler`: Used to process rows one by one without returning a result.

3.7 Can you execute a plain SQL statement with the JDBC template?

Yes, you can execute plain SQL statements using the JDBC template's `execute` method or `update` method, depending on whether the SQL statement is for data modification or retrieval.

3.8 When does the JDBC template acquire (and release) a connection, for every method called or once per template? Why?

The JDBC template typically acquires and releases a connection for every method call by default. This approach ensures that each database operation is isolated in its transaction and that connections are returned to the pool after use, helping to manage resources efficiently and maintain transactional integrity.

3.9 How does the JdbcTemplate support generic queries? How does it return objects and lists/maps of objects?

The JDBC template supports generic queries by allowing you to specify `RowMapper` implementations that map database rows to Java objects. You can define custom `RowMapper` instances for each query, and the JDBC template uses these mappers to return objects or lists/maps of objects, depending on the query result. The returned data can be of any Java class, making it suitable for handling different types of queries.

# 4	Data Management: Transactions

4.1 What is a transaction? What is the difference between a local and a global transaction?

A transaction is a sequence of one or more operations on a database that are treated as a single unit of work. A local transaction is confined to a single resource, like a database, while a global transaction spans multiple resources and can ensure that they are either all committed or all rolled back together.

4.2 Is a transaction a cross-cutting concern? How is it implemented by Spring?

Yes, a transaction is considered a cross-cutting concern because it affects multiple parts of an application. Spring implements transactions using Aspect-Oriented Programming (AOP). It provides transaction management through the `@Transactional` annotation, which allows you to declaratively define transactional behavior.

4.3 How are you going to define a transaction in Spring?

You can define a transaction in Spring by annotating a method with the `@Transactional` annotation. This annotation indicates that the method should run within a transactional context.

4.4 What does @Transactional do? What is the PlatformTransactionManager?

The `@Transactional` annotation in Spring defines the scope of a transaction. It marks a method as transactional, allowing Spring to manage the transaction's beginning, commit, or rollback. The `PlatformTransactionManager` is an interface used to abstract transaction management operations, such as beginning, committing, and rolling back transactions. Different implementations of this interface can be used, depending on the transaction management strategy, such as JDBC or JPA transactions.

4.5 Is the JDBC template able to participate in an existing transaction?

Yes, the JDBC template can participate in an existing transaction. When you use it within a method annotated with `@Transactional`, it will use the existing transaction or create a new one if none exists.

4.6 What is a transaction isolation level? How many do we have, and how are they ordered?

A transaction isolation level defines the degree to which one transaction's changes are visible to other concurrently executing transactions. There are four standard isolation levels in SQL: `READ_UNCOMMITTED`, `READ_COMMITTED`, `REPEATABLE_READ`, and `SERIALIZABLE`. They are ordered from the lowest to the highest level of isolation, with each level providing stricter isolation guarantees.

4.7 What is @EnableTransactionManagement for?

`@EnableTransactionManagement` is used to enable Spring's annotation-driven transaction management. It tells Spring to process annotations like `@Transactional` and configure transactional behavior accordingly.

4.8 What does transaction propagation mean?

Transaction propagation defines how existing transactions are managed when a method annotated with `@Transactional` is called from another method. It determines whether the method should join an existing transaction, create a new one, or run independently.

4.9 What happens if one @Transactional annotated method is calling another @Transactional annotated method on the same object instance?

When one `@Transactional` annotated method calls another `@Transactional` annotated method on the same object instance, the default behavior is that the inner method does not create a new transaction but rather joins the existing transaction, extending its scope.

4.10 Where can the @Transactional annotation be used? What is a typical usage if you put it on a class?

The `@Transactional` annotation can be used at the method level and the class level. When applied at the class level, it sets a default transactional behavior for all methods within the class, which can be overridden by method-level annotations.

4.11 What does declarative transaction management mean?

Declarative transaction management means defining transactional behavior using metadata, such as annotations or XML configuration, rather than writing explicit transaction management code. Spring's `@Transactional` annotation is an example of declarative transaction management.

4.12 What is the default rollback policy? How can you override it?

The default rollback policy in Spring is to roll back the transaction for runtime exceptions (unchecked exceptions) and commit it for checked exceptions. You can override this policy by specifying the `rollbackFor` and `noRollbackFor` attributes in the `@Transactional` annotation to control under which exceptions the transaction should be rolled back or committed.

4.13 What is the default rollback policy in a JUnit test when you use the @RunWith(SpringJUnit4ClassRunner.class) in JUnit 4 or @ExtendWith(SpringExtension.class) in JUnit 5 and annotate your @Test annotated method with @Transactional?

In a JUnit test with Spring, when you use the `@RunWith(SpringJUnit4ClassRunner.class)` in JUnit 4 or `@ExtendWith(SpringExtension.class)` in JUnit 5 and annotate your `@Test` annotated method with `@Transactional`, the default rollback policy is to roll back the transaction after the test method execution. This ensures that test data changes do not affect the database state.

4.14 Why is the term "unit of work" so important, and why does JDBC AutoCommit violate this pattern?

The term "unit of work" is important because it represents a logical operation that should either succeed entirely or fail entirely, maintaining data consistency. JDBC AutoCommit violates this pattern because it commits each SQL statement as a separate transaction, potentially leading to inconsistent or incomplete database changes if an error occurs during a series of SQL statements.

4.15 What do you need to do in Spring if you would like to work with JPA?

To work with JPA in Spring, you need to:
   - Configure a JPA EntityManagerFactory.
   - Set up a DataSource or specify connection properties.
   - Define JPA entities and repositories.
   - Configure transaction management, usually using the `@EnableTransactionManagement` annotation.

4.16 Are you able to participate in a given transaction in Spring while working with JPA?

Yes, you can participate in a given transaction in Spring while working with JPA by annotating methods with `@Transactional`. This allows JPA operations to join the existing transaction or create a new one, depending on the propagation behavior.

4.17 Which PlatformTransaction

Manager(s) can you use with JPA?

You can use the `JpaTransactionManager` as the `PlatformTransactionManager` for managing JPA transactions in Spring applications.

4.18 What do you have to configure to use JPA with Spring? How does Spring Boot make this easier?

To use JPA with Spring, you need to configure an `EntityManagerFactory`, define JPA entities, set up transaction management, and create repositories. Spring Boot simplifies this process by providing auto-configuration and sensible defaults, allowing you to focus on application-specific logic instead of extensive configuration.

# 5	Spring Data JPA

5.1 What is a Repository interface?

A Repository interface, in the context of Spring Data, defines a set of methods for performing common data access operations on a specific domain entity. It provides a high-level, abstracted way to interact with a data store, such as a database, without writing the underlying data access code manually.

5.2 How do you define a Repository interface? Why is it an interface and not a class?

You define a Repository interface by creating an interface that extends one of the Spring Data repository interfaces like `CrudRepository` or `JpaRepository`. It is an interface rather than a class to allow for dynamic proxy-based implementation generation by Spring Data. Spring Data generates the concrete implementation of the repository at runtime, based on the defined methods, eliminating the need for developers to write boilerplate code.

5.3 What is the naming convention for finder methods in a Repository interface?

The naming convention for finder methods in a Repository interface is based on method naming. Spring Data automatically derives the query by analyzing the method name. For example, a method named `findByFirstName(String firstName)` would automatically generate a query to find records with a matching `firstName` property.

5.4 How are Spring Data repositories implemented by Spring at runtime?

Spring Data repositories are implemented by Spring at runtime using dynamic proxy-based implementation generation. Spring Data inspects the methods defined in the Repository interface and generates a concrete implementation that handles data access operations based on method signatures and naming conventions. This implementation delegates to the underlying data store, such as a JPA repository or a MongoDB repository.

5.5 What is @Query used for?

The `@Query` annotation is used in Spring Data repositories to define custom queries using a query language specific to the underlying data store, such as JPQL (Java Persistence Query Language) for JPA repositories or MongoDB queries for MongoDB repositories. It allows developers to write custom query statements and associate them with repository methods, providing flexibility when the automatic query derivation based on method names and conventions is not sufficient.

# 6	Spring MVC and the Web Layer

6.1 MVC is an abbreviation for a design pattern. What does it stand for, and what is the idea behind it?

MVC stands for Model-View-Controller. The idea behind MVC is to separate an application into three interconnected components:
- **Model:** Represents the application's data and business logic.
- **View:** Represents the presentation layer and user interface.
- **Controller:** Manages user input, interacts with the model, and updates the view. It acts as an intermediary between the model and view, facilitating separation of concerns.

6.2 What is the DispatcherServlet, and what is it used for?

The DispatcherServlet is a core component in the Spring MVC framework. It acts as the front controller responsible for handling incoming HTTP requests, dispatching them to the appropriate controllers, and managing the overall request/response flow. It plays a crucial role in routing and managing the processing of web requests in a Spring MVC application.

6.3 What is a web application context? What extra scopes does it offer?

A web application context, represented by `WebApplicationContext`, is a specialized application context in Spring designed for web applications. It inherits the capabilities of the standard application context but provides additional scopes specific to web applications, such as `request`, `session`, and `application` scopes. These scopes allow objects to be stored and accessed at various levels of the web application's lifecycle.

6.4 What is the @Controller annotation used for?

The `@Controller` annotation is used to declare a class as a Spring MVC controller. It identifies a class as a component responsible for processing and handling HTTP requests, typically associated with the presentation layer of a web application.

6.5 Extra: What is @RestController?

The `@RestController` annotation is a specialization of `@Controller` specifically designed for creating RESTful web services. It combines `@Controller` and `@ResponseBody`, indicating that the controller methods return data directly to the response body, often in JSON or XML format.

6.6 How is an incoming request mapped to a controller and mapped to a method?

Incoming requests are mapped to controllers and methods using request mapping annotations like `@RequestMapping`, `@GetMapping`, `@PostMapping`, etc. These annotations define URL patterns or HTTP methods that trigger specific controller methods. The DispatcherServlet analyzes the incoming request, matches it to the appropriate controller and method, and delegates the request for processing.

6.7 What is the difference between @RequestMapping and @GetMapping?

- `@RequestMapping` is a generic annotation used to map HTTP requests to controller methods. It can specify various attributes like the URL path, HTTP methods, request headers, and more.
- `@GetMapping` is a specialization of `@RequestMapping` used specifically for mapping HTTP GET requests to controller methods. It simplifies the mapping of GET requests by focusing on the URL path.

6.8 What is @RequestParam used for?

The `@RequestParam` annotation is used to bind request parameters (query parameters or form data) to method parameters in a controller method. It allows you to extract values from incoming HTTP requests and use them as method arguments.

6.9 What are some of the parameter types for a controller method?

Controller method parameters can include:
- `@RequestParam` for accessing request parameters.
- `@PathVariable` for extracting values from the URL path.
- `@RequestBody` for retrieving the request body as an object.
- `@RequestHeader` for accessing HTTP headers.
- `HttpSession` for managing session attributes.
- `Principal` for accessing the currently authenticated user.

6.10 What other annotations might you use on a controller method parameter? (You can ignore form-handling annotations for this exam)

You might use annotations like `@PathVariable`, `@RequestBody`, `@RequestHeader`, and `@ModelAttribute` on controller method parameters, depending on the specific use case and the data you need to extract from the request.

6.11 What are some of the valid return types of a controller method?

Controller methods can have various return types, including:
- `String`: To specify the name of a view template to render.
- `ModelAndView`: For more control over view rendering and model data.
- `void`: When the response is handled manually.
- JSON or XML data: When building RESTful services using `@ResponseBody`.
- Redirect view names or URLs: For redirecting requests to other URLs or controllers.

# 7	Security

7.1 Extra: Spring Security JSP tag library

The Spring Security JSP tag library provides custom tags that help secure JSP views by controlling access to content based on user roles and authentication status. These tags simplify the integration of security features directly into JSP pages.

7.2 What are authentication and authorization? Which must come first?

Authentication is the process of verifying the identity of a user, typically through credentials like username and password. Authorization, on the other hand, is the process of determining whether an authenticated user has permission to perform a specific action or access a particular resource. Authentication must come before authorization, as you need to know who the user is before deciding what they are allowed to do.

7.3 Is security a cross-cutting concern? How is it implemented internally?

Yes, security is considered a cross-cutting concern because it affects multiple parts of an application. Internally, Spring Security uses Aspect-Oriented Programming (AOP) to handle security aspects, such as authentication and authorization. It intercepts requests, applies security checks, and manages access control to ensure that resources are protected.

7.4 What is the delegating filter proxy?

The DelegatingFilterProxy is a servlet filter provided by Spring Security that delegates filter execution to a Spring bean defined in the application context. It allows Spring Security filters to be configured and managed as Spring beans, making it easier to integrate Spring Security into a web application.

7.5 What is the security filter chain?

The security filter chain is a series of filters responsible for processing HTTP requests and responses in a Spring Security-enabled application. Each filter in the chain performs a specific security-related task, such as authentication, authorization, and session management. The filters are ordered, and requests pass through them in sequence, allowing various security concerns to be addressed in a structured manner.

7.6 Extra: Replacing a Filter, Adding a Filter

You can customize the security filter chain by adding, replacing, or configuring filters based on your application's security requirements. This flexibility allows you to adapt Spring Security to specific use cases.

7.7 What is a security context?

A security context represents the current security state of a user or request within a Spring Security-enabled application. It contains information about the user's authentication status, roles, and granted authorities. The security context is typically stored in a thread-local variable and can be accessed throughout the application to make security-related decisions.

7.8 What does the ** pattern in an antMatcher or mvcMatcher do?

The `**` pattern in an `antMatcher` or `mvcMatcher` is a wildcard that matches any number of path segments, including subdirectories. For example, `"/**"` matches all paths, while `" /admin/**"` matches all paths starting with "/admin/" and any subdirectories.

7.9 Why is the usage of mvcMatcher recommended over antMatcher?

The usage of `mvcMatcher` is recommended over `antMatcher` when configuring security patterns in Spring Security because it aligns with Spring MVC patterns and provides a more intuitive way to define security rules for specific controller endpoints. It simplifies the configuration of security rules in a web application.

7.10 Does Spring Security support password hashing? What is salting?

Yes, Spring Security supports password hashing through various password encoder implementations. Salting is the practice of adding random data (a "salt") to a password before hashing it. This helps protect against attacks like rainbow table attacks because the salt ensures that identical passwords will have different hash values. Spring Security's password encoders can handle salting to enhance security.

7.11 Why do you need method security? What type of object is typically secured at the method level (think of its purpose, not its Java type)?

Method security allows you to control access to specific methods within your application based on user roles and permissions. Typically, at the method level, you secure domain-specific objects or resources that represent business logic, such as services, business methods, or controller actions. Method-level security ensures that only authorized users can invoke these methods.

7.12 What do @PreAuthorized and @RolesAllowed do? What is the difference between them?

- `@PreAuthorized` is an annotation in Spring Security that allows you to specify access control expressions (using SpEL) directly on a method. It checks the specified expression before entering the method.
- `@RolesAllowed` is a Java EE standard annotation that defines the roles permitted to access a method. It is typically used in combination with a Java EE security framework, such as Java EE containers.

The key difference is that `@PreAuthorized` provides more expressive power by allowing complex SpEL expressions to define access control conditions, while `@RolesAllowed` is a simpler annotation focused on role-based access control. In Spring Security applications, `@PreAuthorized` is often preferred for its flexibility.

# 8 REST

8.1 What does REST stand for?

REST stands for Representational State Transfer.

8.2 What is a resource?

In the context of REST, a resource is any piece of information that can be identified and manipulated using a URI (Uniform Resource Identifier). Resources can represent entities like data objects, services, or even abstract concepts.

8.3 What does CRUD mean?

CRUD is an acronym that stands for Create, Read, Update, and Delete. It represents the four basic operations for managing data in a persistent storage system, such as a database.

8.4 Is REST secure? What can you do to secure it?

REST itself is not inherently secure, but security can be implemented within RESTful applications. To secure a REST API, you can use techniques such as authentication, authorization, HTTPS for data encryption, and input validation to protect against common security threats.

8.5 Is REST scalable and/or interoperable?

REST is designed to be scalable and interoperable. It is based on standard HTTP methods and can be easily implemented on various platforms and languages. Its stateless nature and simplicity make it suitable for building scalable and interoperable web services.

8.6 Which HTTP methods does REST use?

REST uses several HTTP methods, including:
- GET: Retrieve data.
- POST: Create new data.
- PUT: Update existing data.
- DELETE: Remove data.
- PATCH: Partially update data.
- OPTIONS: Retrieve information about supported methods.
- HEAD: Retrieve headers of a resource without the body.

8.7 What is an HttpMessageConverter?

An `HttpMessageConverter` in Spring MVC is responsible for converting between HTTP requests and Java objects (serialization and deserialization). It helps translate data from HTTP request and response bodies to Java objects and vice versa. Spring provides various built-in converters for handling different content types like JSON, XML, and more.

8.8 Is REST normally stateless?

Yes, REST is designed to be stateless. Each request from a client to a server must contain all the information necessary for the server to understand and fulfill the request. This statelessness simplifies scalability and allows for easier load balancing.

8.9 What does @RequestMapping do?

The `@RequestMapping` annotation in Spring MVC is used to map HTTP requests to specific controller methods. It defines the URL patterns and HTTP methods that trigger the execution of a controller method when a matching request is received.

8.10 Is @Controller a stereotype? Is @RestController a stereotype? What is a stereotype annotation? What does that mean?

Yes, both `@Controller` and `@RestController` are stereotype annotations in Spring. Stereotype annotations are used to indicate that a class is a specific type of component or bean. In this case, `@Controller` marks a class as a controller, and `@RestController` marks a class as a controller for RESTful services.

8.11 What is the difference between @Controller and @RestController?

- `@Controller` is used for traditional Spring MVC controllers that return views or templates along with data. It is typically used for server-side rendering.
- `@RestController` is used for controllers that primarily return data in a format like JSON or XML, typically for building RESTful APIs. It combines `@Controller` and `@ResponseBody`.

8.12 When do you need @ResponseBody?

You need `@ResponseBody` when you want to indicate that the return value of a controller method should be written directly to the response body instead of being interpreted as a view name to render.

8.13 What are the HTTP status return codes for a successful GET, POST, PUT, or DELETE operation?

- GET: HTTP status code 200 (OK).
- POST: HTTP status code 201 (Created).
- PUT: HTTP status code 200 (OK) or 204 (No Content).
- DELETE: HTTP status code 204 (No Content).

8.14 When do you need @ResponseStatus?

You use `@ResponseStatus` to specify a custom HTTP status code and reason phrase for a controller method's response. It is typically used when you want to define a specific status code other than the default ones.

8.15 Where do you need @ResponseBody? What about @RequestBody? Try not to get these muddled up!

- `@ResponseBody`: It is used at the method level to indicate that the return value should be serialized and included in the HTTP response body.
- `@RequestBody`: It is used at the method parameter level to bind the content of the HTTP request body to a Java object.

8.16 If you saw example Controller code, would you understand what it is doing? Could you tell if it was annotated correctly?

Yes, I can understand and evaluate example Controller code to determine if it is annotated correctly and to understand the functionality it provides.

8.17 Do you need Spring MVC in your classpath?

Yes, you need to include Spring MVC in your classpath when developing web applications using Spring MVC. It provides essential components and functionality for building web-based applications.

8.18 What Spring Boot starter would you use for a Spring REST application?

For a Spring REST application, you can use the `spring-boot-starter-web` starter. It includes dependencies for building web applications, including Spring MVC.

8.19 What are the advantages of the RestTemplate?

The advantages of the `RestTemplate` in Spring include its ease of use for making HTTP requests to remote services, support for various HTTP methods, and the ability to handle response serialization/deserialization, making it

# 9	Testing

9.1 Do you use Spring in a unit test?

In a true unit test, you typically avoid using the full Spring framework and focus on testing individual components (e.g., classes or methods) in isolation. You may not use the Spring container in such tests, but you can use Spring's testing utilities for unit testing, such as `MockBean` or `@Autowired` to inject dependencies for testing.

9.2 What type of tests typically use Spring?

Spring is commonly used in integration tests and end-to-end tests, where you test the interactions between various components or the application as a whole. Spring's testing support is beneficial for writing such tests, including testing the application context and behavior of the entire application.

9.3 How can you create a shared application context in a JUnit integration test?

You can create a shared application context in a JUnit integration test by using the `@ContextConfiguration` annotation along with the `@RunWith(SpringJUnit4ClassRunner.class)` or `@ExtendWith(SpringExtension.class)` annotation in JUnit 5. This setup ensures that the same application context is shared among test methods, allowing you to maintain state between tests.

9.4 Extra Note: @DirtiesContext

The `@DirtiesContext` annotation is used to indicate that a test method or class modifies the application context and may affect other tests. It forces the Spring context to be dirtied and reset after the annotated test method or class, ensuring a clean context for subsequent tests. This is useful when tests modify the application context and could impact the behavior of other tests.

9.5 Extra: Testing with Databases

When testing with databases, you can use tools like an in-memory database (e.g., H2) or test-specific database profiles to isolate test data from production data. Spring Boot simplifies database testing by providing auto-configuration for embedded databases and transaction management.

9.6 When and where do you use @Transactional in testing?

You use `@Transactional` in testing when you want to control the transactional behavior of your tests. It is typically applied at the test method level or class level. When used, the test methods are executed within a transaction that is rolled back after the test method completes, ensuring that the database state is unchanged by the test.

9.7 How are mock frameworks such as Mockito or EasyMock used?

Mock frameworks like Mockito or EasyMock are used to create mock objects or stubs that mimic the behavior of real objects or dependencies. In testing, you can use these mock objects to isolate the code you're testing by replacing real dependencies with mocks. This allows you to control the behavior of dependencies and focus on testing specific parts of your code.

9.8 How is @ContextConfiguration used?

The `@ContextConfiguration` annotation is used to specify the XML configuration files or Java configuration classes that define the Spring application context for a test. You can use it at the class level to define the application context for the entire test class or at the method level to specify the context for a specific test method.

9.9 How does Spring Boot simplify writing tests?

Spring Boot simplifies writing tests by providing auto-configuration for various components, including embedded databases, in-memory servers, and more. It offers testing annotations like `@SpringBootTest` for setting up a test application context and provides default configurations that reduce the need for extensive testing configuration.

9.10 What does @SpringBootTest do? How does it interact with @SpringBootApplication and @SpringBootConfiguration?

- `@SpringBootTest` is used to bootstrap a Spring Boot application context for integration testing. It loads the application context, including the configuration defined in the main application class annotated with `@SpringBootApplication` or `@SpringBootConfiguration`.
- `@SpringBootApplication` or `@SpringBootConfiguration` is typically used in the main application class to define the application's configuration. When testing with `@SpringBootTest`, it ensures that the main application's configuration is included in the test application context, allowing you to test the application in a similar context as it runs in production.

# 10	Spring Boot Intro

10.1 What is Spring Boot?

Spring Boot is an open-source framework and project within the Spring ecosystem that simplifies the development of production-ready applications based on the Spring framework. It provides a set of conventions, auto-configuration, and ready-to-use templates to streamline application setup and development.

10.2 What are the advantages of using Spring Boot?

The advantages of using Spring Boot include:
- Simplified setup and configuration.
- Built-in support for common tasks like database access, security, and messaging.
- Production-ready features like health checks and metrics.
- Easy integration with various data sources and embedded servers.
- Reduced boilerplate code, leading to faster development.
- A vibrant community and extensive documentation.

10.3 Why is it “opinionated”?

Spring Boot is "opinionated" because it enforces a set of defaults and conventions that guide developers to follow best practices for building Spring applications. These opinions help standardize application development, reduce decision fatigue, and encourage consistency across Spring Boot projects.

10.4 What things affect what Spring Boot sets up?

Several factors affect what Spring Boot sets up for an application:
- The project's dependencies and the Spring Boot starter POMs used.
- The configuration provided in property files or YAML files.
- The presence of specific classes and annotations in the codebase.
- The profiles activated during application startup.

10.5 Extra: Auto-Configuration Example

Auto-configuration in Spring Boot automatically configures beans and components based on the project's dependencies and the environment. It simplifies configuration by providing sensible defaults. For example, if Spring Boot detects the presence of a database driver on the classpath, it auto-configures a data source bean.

10.6 What is a Spring Boot starter POM? Why is it useful?

A Spring Boot starter POM is a pre-defined set of dependencies that simplifies project setup by providing a curated list of libraries and configurations for common use cases. Starters are useful because they reduce the need to manually add dependencies, making it easier to set up projects tailored to specific requirements, such as web applications or data access.

10.7 Spring Boot supports both properties and YML files. Would you recognize and understand them if you saw them?

Yes, I would recognize and understand both property files (`.properties`) and YAML files (`.yml`) used for configuring Spring Boot applications. Property files use key-value pairs, while YAML files use a more structured, human-readable format with indentation to define configuration properties.

10.8 Can you control logging with Spring Boot? How?

Yes, Spring Boot allows you to control logging through properties or YAML configuration files. You can configure log levels, log output formats, and more by specifying properties like `logging.level`, `logging.file`, and `logging.pattern.console` in your configuration files.

10.9 Where does Spring Boot look for property file by default?

By default, Spring Boot looks for property files (`application.properties` or `application.yml`) in the classpath. It searches for these files in the `src/main/resources` directory of your project.

10.10 How do you define profile-specific property files?

You can define profile-specific property files by using the naming convention `application-{profile}.properties` or `application-{profile}.yml`, where `{profile}` is the name of the active profile. For example, to define properties for a "development" profile, create a file named `application-development.properties` or `application-development.yml`.

10.11 How do you access the properties defined in the property files?

You can access the properties defined in property files using the `@Value` annotation to inject them into Spring beans or components. Alternatively, you can use the `Environment` object to programmatically retrieve properties in your application code.

10.12 What properties do you have to define in order to configure external MySQL?

To configure an external MySQL database in Spring Boot, you typically need to define properties like `spring.datasource.url`, `spring.datasource.username`, and `spring.datasource.password` in your property or YAML configuration file. You may also specify the JDBC driver class using `spring.datasource.driver-class-name`.

10.13 How do you configure default schema and initial data?

You can configure the default schema and initial data in Spring Boot by using the `spring.datasource.schema` and `spring.datasource.data` properties. You can provide SQL scripts or file paths to these properties to set up the schema and insert initial data during application startup.

10.14 Extra: How to customize the Spring banner?

You can customize the Spring Boot banner by creating a text or image file named `banner.txt` or `banner.png` and placing it in the `src/main/resources` directory of your project. Spring Boot will display your custom banner during application startup.

10.15 What is a fat jar? How is it different from the original jar?

A fat jar (or uber-jar) is a self-contained JAR (Java Archive) file that contains not only your application's compiled classes and resources but also all of its dependencies and libraries. This allows you to package your application as a single executable JAR, simplifying deployment. In contrast, the original JAR typically contains only your application code and relies on external dependencies.

10.16 What is the difference between an embedded container and a WAR?

An embedded container is a web server that is bundled with your application, allowing it to run as a standalone application without requiring a separate external web server like Apache Tomcat. A WAR (Web Application Archive) is a deployable package that contains web resources and is typically deployed to an external web server or servlet container. Spring Boot supports both embedded containers (e.g., Tomcat, Jetty) and WAR deployments, depending on your project's requirements.

10.17 What embedded containers does Spring Boot support?

Spring Boot supports various embedded containers, including Apache Tomcat, Jetty

, and Undertow. You can choose the embedded container that best fits your application's needs, and Spring Boot will auto-configure it accordingly.

# 11	Spring Boot Auto Configuration

11.1 How does Spring Boot know what to configure?

Spring Boot uses a combination of classpath scanning, conditional annotations, and the presence of specific libraries and properties to determine what to configure. It scans the classpath for classes and libraries and checks for certain conditions and conventions to auto-configure components.

11.2 What does @EnableAutoConfiguration do?

The `@EnableAutoConfiguration` annotation is a meta-annotation that enables Spring Boot's auto-configuration feature. It triggers the auto-configuration process, where Spring Boot automatically configures beans and components based on the project's dependencies and conditions.

11.3 What does @SpringBootApplication do?

`@SpringBootApplication` is a composite annotation that combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. It is commonly used in the main application class and serves as a convenient way to enable Spring Boot features, configure the application, and scan for components.

11.4 Does Spring Boot do component scanning? Where does it look by default?

Yes, Spring Boot performs component scanning by default. It looks for components (e.g., `@Controller`, `@Service`, `@Repository`, `@Component`) in the same package as the main application class and its sub-packages. This default behavior can be overridden by specifying a different base package in the `@SpringBootApplication` annotation.

11.5 How are DataSource and JdbcTemplate auto-configured?

Spring Boot auto-configures the `DataSource` and `JdbcTemplate` beans based on the presence of specific database-related libraries on the classpath. For example, if it detects the H2 database driver, it configures an H2 data source. You can customize data source properties in your application's configuration files.

11.6 What is the spring.factories file for?

The `spring.factories` file is a key part of Spring Boot's auto-configuration mechanism. It is a properties file that lists the auto-configuration classes to be enabled for various conditions and scenarios. These classes define how auto-configuration should work based on the presence of specific libraries and properties.

11.7 Extra: Background information on Auto Configuration

Auto-configuration in Spring Boot is based on the principle of "sensible defaults." It aims to simplify the setup of Spring applications by providing default configurations that can be easily overridden or customized. Auto-configuration classes are conditional and only activate when certain conditions are met.

11.8 How do you customize Spring auto-configuration?

You can customize Spring Boot's auto-configuration by providing your own configurations or beans. To do this, create a configuration class annotated with `@Configuration` and define the custom beans or configurations. Spring Boot will use your custom configurations in place of its auto-configured defaults.

11.9 What are the examples of @Conditional annotations? How are they used?

The `@Conditional` annotations in Spring Boot allow you to conditionally enable or disable configuration classes or beans based on certain conditions. Examples include:
- `@ConditionalOnClass`: Configures a bean if a specified class is present on the classpath.
- `@ConditionalOnMissingClass`: Configures a bean if a specified class is not present on the classpath.
- `@ConditionalOnProperty`: Configures a bean based on the presence or absence of a specific property in the configuration files.
- `@ConditionalOnBean`: Configures a bean if a specified bean is present in the application context.
- `@ConditionalOnMissingBean`: Configures a bean if a specified bean is not present in the application context.

These annotations allow you to control when certain configurations should be applied, making it easier to customize the behavior of your Spring Boot application.

# 12	Spring Boot Actuator

12.1 Extra: Getting started

Getting started with Spring Boot Actuator involves adding the `spring-boot-starter-actuator` dependency to your project's build configuration. This starter includes the Actuator module, which provides various production-ready features for monitoring and managing your application.

12.2 What value does Spring Boot Actuator provide?

Spring Boot Actuator provides valuable features for monitoring and managing Spring Boot applications in a production environment. It offers endpoints to access runtime information, health checks, metrics, and more. These features are essential for maintaining the health and performance of your application.

12.3 What are the two protocols you can use to access actuator endpoints?

Spring Boot Actuator endpoints can be accessed using two protocols:
- HTTP/HTTPS: You can access endpoints over HTTP/HTTPS by making HTTP requests to the respective endpoint URLs.
- JMX (Java Management Extensions): You can access endpoints programmatically using JMX, which provides a Java-based management and monitoring interface.

12.4 What are the actuator endpoints that are provided out of the box?

Spring Boot Actuator provides several built-in endpoints out of the box, including:
- `/health`: Provides application health information.
- `/info`: Displays arbitrary application information.
- `/metrics`: Exposes metrics related to the application's performance.
- `/env`: Displays environment properties.
- `/beans`: Lists all Spring beans in the application context.
- `/mappings`: Shows all request mappings.
- `/actuator`: Serves as a root endpoint for Actuator features.

12.5 What is the info endpoint for? How do you supply data?

The `/info` endpoint in Spring Boot Actuator is used to provide arbitrary application information. You can supply data for this endpoint by defining properties in your application's configuration files or by programmatically setting the properties using the `ManagementEndpointProperties` bean.

12.6 How do you change the logging level of a package using the loggers endpoint?

You can change the logging level of a package using the `/actuator/loggers` endpoint. To modify the logging level, send a POST request to this endpoint with a JSON payload specifying the logger name and the desired log level.

12.7 How do you access an endpoint using a tag?

To access an endpoint using a tag, you can include the tag as a URL parameter when making an HTTP request to the endpoint. For example, to access the `/metrics` endpoint with a specific tag, you can append `?tag=mytag` to the endpoint URL.

12.8 What is metrics for?

The `/metrics` endpoint in Spring Boot Actuator provides access to various metrics related to the application's performance and resource utilization. It allows you to monitor and analyze key metrics, such as memory usage, garbage collection statistics, HTTP request counts, and custom application-specific metrics.

12.9 How do you create a custom metric with or without tags?

You can create custom metrics in Spring Boot Actuator with or without tags using the `MeterRegistry` API provided by the Micrometer library. To create a custom metric without tags, you can simply register a meter for the desired metric name. To create a custom metric with tags, you can specify additional tags when registering the meter.

12.10 Extra: Controller metric with @Timed

You can use the `@Timed` annotation from the Micrometer library to instrument a Spring MVC controller method and collect metrics for its execution time. This annotation allows you to monitor the performance of specific controller endpoints.

12.11 What is a Health Indicator?

A Health Indicator in Spring Boot Actuator is a component responsible for providing information about the health status of various aspects of your application. Health Indicators are used to perform checks on application dependencies, databases, external services, and more, and report their health status.

12.12 What are the Health Indicators that are provided out of the box?

Spring Boot Actuator provides several built-in Health Indicators out of the box, including:
- `DiskSpaceHealthIndicator`: Checks the available disk space.
- `PingHealthIndicator`: Pings a specified endpoint to determine health.
- `DataSourceHealthIndicator`: Checks the health of data sources.
- `MongoHealthIndicator`: Checks the health of MongoDB.
- `RabbitHealthIndicator`: Checks the health of RabbitMQ.
- `RedisHealthIndicator`: Checks the health of Redis.

12.13 What is the Health Indicator status?

The Health Indicator status represents the health condition of a specific aspect or dependency of the application. It is reported as one of the following statuses: `UP` (healthy), `DOWN` (unhealthy), `OUT_OF_SERVICE` (temporarily out of service), or `UNKNOWN` (status cannot be determined).

12.14 What are the Health Indicator statuses that are provided out of the box?

Out of the box, Spring Boot Actuator provides the following Health Indicator statuses: `UP`, `DOWN`, `OUT_OF_SERVICE`, and `UNKNOWN`. These statuses help you assess the health of various components and services in your application.

12.15 How do you change the Health Indicator status severity order?

You can change the Health Indicator status severity order by configuring the `management.health.status.order` property in your application's configuration. This property allows you to customize the order in which the statuses are evaluated and reported.

12.16 Why do you want to leverage a 3rd-party external monitoring system?

Leveraging a third-party external monitoring system is important for several reasons:
- Comprehensive Monitoring: External monitoring systems offer a broader range of monitoring capabilities, including alerting, historical data storage, and visualization.
- Scalability: External systems can handle monitoring for multiple applications and services, providing centralized monitoring and management.
- Integration: They can integrate with various monitoring and alerting tools, making it easier to manage and respond to incidents.
- Specialized Features: External systems often provide specialized features for performance optimization, anomaly detection, and predictive analysis.
- Long-Term Data Storage: External systems are designed to store monitoring data for extended periods, facilitating trend analysis and capacity planning.

Using an external monitoring system complements Spring Boot Actuator's built-in capabilities and ensures comprehensive monitoring and management of your application in production.

# 13	Spring Boot Testing

13.1 Extra: What are the Spring Boot Testing basics?

Spring Boot testing basics involve using various testing annotations and utilities provided by Spring Boot to simplify the testing process. These annotations and utilities enable you to write unit tests, integration tests, and end-to-end tests for your Spring Boot applications.

13.2 When do you want to use @SpringBootTest annotation?

You want to use the `@SpringBootTest` annotation when you need to perform integration testing of your Spring Boot application. It loads the complete Spring application context, allowing you to test the behavior of your application with all its components and configurations in place.

13.3 What does @SpringBootTest auto-configure?

`@SpringBootTest` auto-configures the Spring Boot application context and loads all beans, components, and configurations defined in your application. It also auto-configures embedded web servers (if needed) and sets up properties from `application.properties` or `application.yml` for testing.

13.4 What dependencies does spring-boot-starter-test bring to the classpath?

The `spring-boot-starter-test` dependency brings several testing-related dependencies to the classpath, including JUnit, Spring Test, Hamcrest, and Mockito. These libraries are commonly used for writing unit tests and integration tests in Spring Boot applications.

13.5 How do you perform integration testing with @SpringBootTest for a web application?

To perform integration testing for a web application using `@SpringBootTest`, you can use the `TestRestTemplate` or `WebTestClient` provided by Spring Boot. These tools allow you to send HTTP requests to your application's endpoints and validate the responses. You can also use annotations like `@AutoConfigureMockMvc` to test web controllers.

13.6 When do you want to use @WebMvcTest? What does it auto-configure?

You want to use the `@WebMvcTest` annotation when you need to perform unit testing for a specific web controller in your Spring Boot application. It auto-configures a minimal Spring application context that focuses on the web layer, allowing you to test the behavior of web controllers while excluding other components and configurations.

13.7 What are the differences between @MockBean and @Mock?

- `@MockBean` is used in integration tests with Spring Boot and is typically applied to a Spring bean. It replaces or mocks the bean in the application context, allowing you to control its behavior during testing.
- `@Mock` is part of the Mockito library and is used in unit tests. It creates a mock object of a specific class or interface, but it does not interact with the Spring application context. It is mainly used for unit testing components in isolation.

13.8 When do you want @DataJpaTest for? What does it auto-configure?

You want to use the `@DataJpaTest` annotation when you need to perform unit or integration testing of your JPA repositories in a Spring Boot application. It auto-configures a minimal Spring application context that focuses on JPA-related components and sets up an in-memory database for testing. This annotation is useful for testing database interactions and repository methods in isolation.
