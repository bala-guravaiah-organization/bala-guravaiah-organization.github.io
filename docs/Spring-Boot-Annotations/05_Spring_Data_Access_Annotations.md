
#### Data Access Annotations

| No. | Annotation | Description |
|----|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1 | **@Entity** | Marks a class as a JPA entity, mapped to a database table. |
| 2 | **@Table** | Specifies the database table to which an entity is mapped. |
| 3 | **@Id** | Marks a field as the primary key in an entity. |
| 4 | **@GeneratedValue** | Specifies the strategy for generating primary key values. |
| 5 | **@Column** | Maps a field to a specific database column. |
| 6 | **@OneToOne** | Defines a one-to-one relationship between entities. |
| 7 | **@OneToMany** | Defines a one-to-many relationship between entities. |
| 8 | **@ManyToOne** | Defines a many-to-one relationship between entities. |
| 9 | **@ManyToMany** | Defines a many-to-many relationship between entities. |
| 10 | **@JoinColumn** | Specifies the column to join in a relationship. |
| 11 | **@JoinTable** | Specifies the join table for a many-to-many relationship. |
| 12 | **@Transactional** | Manages transactions in JPA operations. |
| 13 | **@Query** | Defines custom JPQL or SQL queries for repository methods. |
| 14 | **@NamedQuery** | Declares a static, named query at the entity level. |
| 15 | **@EntityListeners** | Specifies listener classes for entity lifecycle events. |
| 16 | **@Embeddable** | Marks a class that can be embedded in another entity. |
| 17 | **@Embedded** | Marks a field as embedded (referring to an `@Embeddable` type). |

