#### Core Annotations

| No. | Annotation | Description |
|----|----------------------|--------------------------------------------------------------------------------------------------------------------------------|
| 1  | **@SpringBootApplication** | Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. Entry point for Spring Boot applications. |
| 2  | **@Configuration** | Marks a class as a source of bean definitions for the application context. |
| 3  | **@Bean** | Indicates a method produces a bean managed by the Spring container. |
| 4  | **@Component** | Marks a class as a Spring-managed component (generic stereotype for any Spring-managed bean). |
| 5  | **@Service** | Specialization of `@Component` for service-layer classes. |
| 6  | **@Repository** | Specialization of `@Component` for persistence-layer classes. Automatically translates exceptions into Spring’s data access exceptions. |
| 7  | **@Controller** | Indicates that a class handles web requests (used in MVC). |
| 8  | **@RestController** | Combines `@Controller` and `@ResponseBody`. Used to handle REST APIs. |

