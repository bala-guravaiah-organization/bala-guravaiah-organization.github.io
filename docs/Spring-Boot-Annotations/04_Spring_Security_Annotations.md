#### Security Annotations

| No. | Annotation | Description |
|----|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1 | **@EnableWebSecurity** | Enables Spring Security's web security features for the application. |
| 2 | **@EnableGlobalMethodSecurity** | Enables method-level security annotations like `@PreAuthorize` and `@Secured`. |
| 3 | **@Secured** | Marks a method or class to restrict access based on roles or authorities. |
| 4 | **@PreAuthorize** | Provides access control based on expressions, allowing method-level security using SpEL (Spring Expression Language). |
| 5 | **@PostAuthorize** | Allows access control after a method is executed, useful for restricting access based on the method's return value. |
| 6 | **@RolesAllowed** | Restricts access to a method or class based on specified roles. |
| 7 | **@PreFilter** | Filters input parameters before a method is executed. Useful for restricting data based on security attributes. |
| 8 | **@PostFilter** | Filters output parameters after a method is executed. Useful for restricting data based on security attributes. |
| 9 | **@Authenticated** | Indicates that a user must be authenticated to access the annotated method or class. |
| 10 | **@EnableOAuth2Sso** | Enables Single Sign-On (SSO) using OAuth2 for a Spring Boot application, typically with services like Google or Facebook. |
| 11 | **@EnableResourceServer** | Marks the application as a resource server for OAuth2, used for securing REST APIs. |
| 12 | **@EnableAuthorizationServer** | Configures the application as an OAuth2 authorization server, managing tokens and authorization flows. |
| 13 | **@AuthenticationPrincipal** | Binds the currently authenticated principal (usually the user details) to a method parameter. |
| 14 | **@EnableGlobalAuthentication** | Enables global authentication settings, allowing custom authentication logic across the entire application. |

