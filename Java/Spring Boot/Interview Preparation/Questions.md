# Spring Boot Overview

**Spring Boot** is a Java framework that makes it easier to create and run Java applications. It simplifies the configuration and setup process, allowing developers to focus on writing application code. As a module of the Spring Framework, Spring Boot facilitates **Rapid Application Development (RAD)**.

---

## Why Spring Boot?

### Features:
- **Configuration Simplification**: Automatically configures Spring modules and components based on dependencies.
- **Dependency Management**: Automatically handles transitive dependencies after they are added.
- **Embedded Servers**: Provides embedded servers like **Tomcat**, **Jetty**, and **Undertow**.
- **Production-Ready Features**: Includes **metrics**, **health checks**, and other operational tools.
- **Autoconfiguration**: Automatically configures dependencies like `Spring Data JPA`.

### Advantages Over Spring Framework:
- **Ease of Use**: Removes boilerplate code and simplifies application setup.
- **Rapid Development**: Opinionated approach and auto-configuration help developers quickly build applications.
- **Dependency Management**: Pre-configured dependencies for various functionalities.
- **Embedded Servers**: No need for an external server to run your application.

---

## How Spring Boot Works

1. **Scanning Starter Dependencies**: Spring Boot scans the dependencies listed in your `pom.xml` (Maven) or `build.gradle` (Gradle).
2. **Auto-Configuration**: Automatically configures modules based on the detected dependencies.
3. **Application Startup**:
   - The `main()` method is executed.
   - The `run()` method of `SpringApplication` creates and initializes the application context.
4. **Embedded Server Initialization**: Starts the embedded server (e.g., Tomcat) to run your application.

---

## Key Annotations in Spring Boot

- **`@SpringBootApplication`**: Combines three annotations:
  - `@Configuration`: Marks a class as a source of bean definitions.
  - `@EnableAutoConfiguration`: Enables Spring Boot’s auto-configuration mechanism.
  - `@ComponentScan`: Scans for Spring components in the specified package.
  
- **`@Component`**: Marks a class as a Spring-managed bean.
- **`@Autowired`**: Automatically injects dependencies where required.
- **`@Service`**: Indicates a service layer component.
- **`@RestController`**: Marks a class as a RESTful web service controller.
- **`@RequestMapping`**: Maps specific HTTP requests to methods.
- **`@Repository`**: Specializes `@Component` for database access layers.

---

## What are Spring Boot Starters?

**Spring Boot Starters** are pre-configured dependencies that simplify adding functionality to Spring Boot applications. They include:
1. Dependency management.
2. Version control.
3. Configuration for specific use cases.

### Key Dependencies:
1. **`spring-boot-starter-parent`**: Provides default configuration for Spring applications (e.g., dependency versions, compiler levels, Maven plugins).
2. **`spring-boot-maven-plugin`**: Enables packaging and running Spring Boot applications.
3. **`spring-boot-starter-test`**: Provides libraries for testing.
4. **`spring-boot-starter-security`**: Adds Spring Security support.

---

## Spring Boot CLI

**Spring Boot CLI** is a command-line tool that simplifies the creation, running, and management of Spring Boot applications.

---

## What is Thymeleaf?

**Thymeleaf** is a Java-based server-side templating engine used to render dynamic web pages in Spring Boot applications.

---
****
