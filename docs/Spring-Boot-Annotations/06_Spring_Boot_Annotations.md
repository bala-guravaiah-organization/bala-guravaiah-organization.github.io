
| No. | Annotation | Description |
|----|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1 | **@Repository** | Indicates that a class is a Spring-managed repository for data access. |
| 2 | **@Autowired** | Injects the required dependencies into Spring beans, including JDBC repositories. |
| 3 | **@EnableJpaRepositories** | Enables Spring Data JPA repositories. |
| 4 | **@EnableTransactionManagement** | Enables transaction management in Spring applications. |
| 5 | **@QueryParam** | Binds query parameters to method arguments. |
| 6 | **@Param** | Binds method parameters in a named query to method arguments. |
| 7 | **@Transactional(readOnly=true)** | Specifies that the transaction is read-only for performance optimization. |
| 8 | **@EnableBatchProcessing** | Enables Spring Batch configuration for batch processing jobs. |
| 9 | **@Document** | Marks a class as a MongoDB document. |
| 10 | **@Field** | Maps a field in a MongoDB document to a specific field in the class. |
| 11 | **@Id** | Marks the primary key field in MongoDB. |

| Classification                  | Annotation                                     | Description |
|--------------------------------|---------------------------------|-------------|
| **Spring Redis Annotations**   | @RedisHash                      | Marks a class as a Redis hash. |
|                                | @Id                              | Marks the primary key for Redis data. |
|                                | @Indexed                         | Marks a field to be indexed in Redis. |
| **Spring Couchbase Annotations** | @Document                        | Marks a class as a Couchbase document. |
|                                | @Id                              | Marks the primary key for Couchbase documents. |
| **Spring SQL Annotations**     | @NamedNativeQuery                | Declares a native SQL query at the entity level. |
|                                | @Query                           | Defines a custom SQL query for the repository method. |
| **Spring Transaction Annotations** | @EnableTransactionManagement       | Enables Spring’s declarative transaction management. |
|                                | @Transactional(propagation=Propagation.REQUIRES_NEW) | Begins a new transaction, suspending the current one. |
|                                | @Transactional(isolation=Isolation.SERIALIZABLE) | Sets the isolation level for the transaction. |


