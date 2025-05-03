#### Web and HTTP Annotations

| No. | Annotation | Description |
|----|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1 | **@RequestMapping** | Maps HTTP requests to handler methods or classes, supporting all HTTP methods. |
| 2 | **@GetMapping** | Shortcut for HTTP GET requests. |
| 3 | **@PostMapping** | Shortcut for HTTP POST requests. |
| 4 | **@PutMapping** | Shortcut for HTTP PUT requests. |
| 5 | **@DeleteMapping** | Shortcut for HTTP DELETE requests. |
| 6 | **@PatchMapping** | Shortcut for HTTP PATCH requests. |
| 7 | **@RequestBody** | Binds the HTTP request body to a method parameter, allowing automatic deserialization. |
| 8 | **@RequestParam** | Extracts query parameters from the request URL and binds them to method parameters. |
| 9 | **@RequestHeader** | Binds a request header to a method parameter in the controller. |
| 10 | **@PathVariable** | Extracts values from the URL path and binds them to method parameters. |
| 11 | **@ResponseBody** | Indicates that the return value of a method should be written directly to the HTTP response body. |
| 12 | **@CrossOrigin** | Configures Cross-Origin Resource Sharing (CORS) for a resource or endpoint, enabling cross-origin requests. |
| 13 | **@CookieValue** | Binds a cookie value to a method parameter in the controller. |
| 14 | **@ModelAttribute** | Binds request parameters to a model object, allowing for easier population of form objects. |
| 15 | **@SessionAttributes** | Specifies model attributes that should be stored in the session. |
| 16 | **@InitBinder** | Allows customizing the binding of web request parameters to method arguments, typically used for custom property editors. |

