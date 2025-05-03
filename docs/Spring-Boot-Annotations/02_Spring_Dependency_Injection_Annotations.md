#### Dependency Injection Annotations

| No. | Annotation | Description |
|----|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1  | **@Autowired** | Automatically injects a bean by type into the field, constructor, or method. |
| 2 | **@Qualifier** | Specifies the bean to inject when multiple candidates are available, used with `@Autowired`. |
| 3 | **@Primary** | Marks a bean as the primary candidate when multiple beans of the same type are available for injection. |
| 4 | **@Value** | Injects values from property files or environment variables into fields, methods, or constructors. |
| 5 | **@Scope** | Specifies the scope of a bean (`singleton`, `prototype`, `request`, `session`, etc.). |

