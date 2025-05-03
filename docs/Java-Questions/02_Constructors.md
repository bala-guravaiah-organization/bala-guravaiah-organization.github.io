
#### Can a constructor be private? In which scenarios is it useful?
 
Yes, a constructor can be `private` in Java. This is mainly used in:  
1. **Singleton Design Pattern:** Ensures only one instance of a class exists.  
2. **Factory Methods:** Restricts direct object creation and allows controlled instantiation via static methods.  
3. **Static Utility Classes:** Prevents instantiation of classes like `Math` or `Collections`.  

#### **Example: Singleton Class with a Private Constructor**  
```java
class Singleton {
    private static Singleton instance;
    
    private Singleton() { // Private constructor
        System.out.println("Instance created");
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
public class Test {
    public static void main(String[] args) {
        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();
        System.out.println(obj1 == obj2); // true
    }
}
```
**Output:**  
```
Instance created  
true
```

#### **Cross-questions you might face:**  
1. **What happens if you try to create an object of a class with a private constructor?**  
   → Compilation error if there’s no static method to access it.  
2. **Can an inner class access a private constructor of the outer class?**  
   → Yes, inner classes can access private members of the outer class.  

---

#### What is the use of the `super` keyword? How is it utilized in a Spring Boot microservices architecture?
 
`super` is used to refer to the immediate parent class members (variables, methods, or constructors).  
- **Access Parent Class Method:** `super.methodName();`  
- **Access Parent Class Constructor:** `super();`  
- **Access Parent Class Variable:** `super.variableName;`  

##### **Example: Using `super` to Call Parent Constructor**
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}
class Child extends Parent {
    Child() {
        super(); // Calls Parent constructor
        System.out.println("Child constructor");
    }
}
public class Test {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```
**Output:**  
```
Parent constructor  
Child constructor
```

#### **Usage in Spring Boot Microservices:**  
In Spring Boot, `super` is used in:  
- **Extending `JpaRepository` in Repository classes**  
- **Calling Parent `@Service` or `@Component` Methods**  
- **Extending Base Controller or Exception Handlers**  

#### **Cross-questions you might face:**  
1. **What happens if you don’t use `super()` in a constructor?**  
   → The compiler automatically adds `super();` unless the parent class has no default constructor.  
2. **Can `super` be used inside a static method?**  
   → No, `super` refers to instance-level members.  

---

#### How does an interface differ from an abstract class? Provide an example where you implemented an interface in your project.
 
| Feature              | Interface | Abstract Class |
|----------------------|-----------|---------------|
| Methods             | Only abstract methods (until Java 8) | Can have both abstract & concrete methods |
| Variables           | Only static & final | Can have instance variables |
| Multiple Inheritance | Yes | No |
| Constructors        | Not allowed | Allowed |

#### **Example: Implementing an Interface**
```java
interface Payment {
    void processPayment(double amount);
}
class CreditCardPayment implements Payment {
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment: $" + amount);
    }
}
public class Test {
    public static void main(String[] args) {
        Payment payment = new CreditCardPayment();
        payment.processPayment(100);
    }
}
```
**Output:**  
```
Processing credit card payment: $100
```

#### **Cross-questions you might face:**  
1. **Can an interface have a constructor?**  
   → No, because interfaces don’t have instance variables.  
2. **How are default methods in interfaces useful?**  
   → They allow backward compatibility without breaking existing implementations.  

---

#### Can a Java class implement multiple interfaces? How does Spring handle multiple interface implementations in dependency injection?
 
Yes, a Java class can implement multiple interfaces.  
```java
interface A {
    void methodA();
}
interface B {
    void methodB();
}
class C implements A, B {
    public void methodA() { System.out.println("A method"); }
    public void methodB() { System.out.println("B method"); }
}
```
#### **How Spring Handles Multiple Implementations?**  
- **@Primary:** Marks one implementation as the default.  
- **@Qualifier:** Specifies which implementation should be injected.  

```java
@Component
public class CreditPayment implements Payment { /* Implementation */ }

@Component
@Primary
public class DebitPayment implements Payment { /* Implementation */ }

@Service
public class PaymentService {
    @Autowired
    private Payment payment; // Injects DebitPayment due to @Primary
}
```

#### **Cross-questions you might face:**  
1. **What happens if two beans implement the same interface and neither has `@Primary`?**  
   → Spring throws `NoUniqueBeanDefinitionException`.  
2. **Can an interface extend another interface?**  
   → Yes, an interface can extend multiple interfaces.  

---

#### How to use `this()` to call another constructor inside the same class?

In Java, `this()` is used to call another constructor within the same class. This helps in constructor chaining and avoids code duplication.

#### **Example:**
```java
class Person {
    String name;
    int age;

    Person() {
        this("Unknown", 0); // Calls parameterized constructor
    }

    Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person();
        Person p2 = new Person("John", 25);
    }
}
```
**Output:**
```
Name: Unknown, Age: 0
Name: John, Age: 25
```
#### Using `this` in a Spring Boot Project
 
In a Spring Boot project, `this` is often used to refer to the current instance within a class, primarily in:
1. **Dependency Injection in Constructors**
2. **Updating Model Objects**
3. **Fluent APIs for Method Chaining**

#### **1. Using `this` in Constructor Injection**
Spring Boot allows constructor-based dependency injection. `this` can be used to differentiate between instance variables and constructor parameters.
```java
import org.springframework.stereotype.Service;

@Service
public class UserService {
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository; // `this` differentiates instance variable
    }
}
```

#### **2. Using `this` in Model Objects**
`this` is used inside entity classes to return the current instance, enabling fluent API usage.
```java
import jakarta.persistence.*;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    
    public User setName(String name) {
        this.name = name;
        return this; // Enables method chaining
    }
}
```

#### **3. Using `this` in Method Calls**
When calling another method within the same class, `this` ensures clarity.
```java
@Service
public class NotificationService {
    public void sendEmail(String message) {
        this.logMessage(message); // Explicitly calling another method in the same class
    }
    
    private void logMessage(String message) {
        System.out.println("Logging: " + message);
    }
}
```

#### **Best Practices:**
✔ Use `this` when necessary to avoid ambiguity.
✔ Avoid excessive use if context is already clear.
✔ Prefer constructor-based injection over field injection for better testability.

#### Can `this` Be Used in a Static Method?

No, `this` cannot be used in a static method because `this` refers to the current instance of the class, and static methods belong to the class rather than an instance.

#### **Example of Incorrect Usage:**
```java
class Example {
    static void staticMethod() {
        System.out.println(this); // Compilation Error!
    }
}
```
**Error:** `Cannot use 'this' in a static context.`

#### **Why?**
- Static methods are associated with the class itself, not an instance.
- `this` refers to an object instance, which does not exist inside a static method.

#### **Alternative Solution:**
If you need an instance reference inside a static method, create an object:
```java
class Example {
    void instanceMethod() {
        System.out.println("Instance method called");
    }
    
    static void staticMethod() {
        Example obj = new Example();
        obj.instanceMethod(); // Correct way to call an instance method
    }
}
```

#### **Key Takeaways:**
✔ `this` can only be used in instance methods or constructors.
✔ Use object references inside static methods if an instance is needed.
✔ Static methods are meant for operations independent of instance data.

#### How to Use `super()` to Call a Parent Class Constructor?


In Java, `super()` is used to call a constructor of the immediate parent class. This is helpful for initializing parent class properties before executing child class logic.

#### **Example: Calling Parent Constructor**
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // Calls Parent class constructor
        System.out.println("Child constructor");
    }
}

public class Test {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```
**Output:**
```
Parent constructor
Child constructor
```

#### **Key Points:**
✔ `super()` must be the first statement in a constructor.  
✔ If a parent class has a parameterized constructor, you must explicitly call `super(args)`.  
✔ If no constructor is defined in the parent class, Java provides a default constructor.

#### **Example: Calling Parameterized Parent Constructor**
```java
class Parent {
    Parent(String name) {
        System.out.println("Parent constructor: " + name);
    }
}

class Child extends Parent {
    Child() {
        super("John"); // Calls Parent's parameterized constructor
        System.out.println("Child constructor");
    }
}
```
**Output:**
```
Parent constructor: John
Child constructor
```

✔ Use `super(args)` to pass parameters to the parent constructor. 
✔ If a parent has only parameterized constructors, you must call one explicitly using `super()`.

#### How to Use `super()` to Call a Parent Class Constructor?

### **Answer:**
In Java, `super()` is used to call a constructor of the immediate parent class. This is helpful for initializing parent class properties before executing child class logic.

#### **Example: Calling Parent Constructor**
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // Calls Parent class constructor
        System.out.println("Child constructor");
    }
}

public class Test {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```
**Output:**
```
Parent constructor
Child constructor
```

#### **Key Points:**
✔ `super()` must be the first statement in a constructor.  
✔ If a parent class has a parameterized constructor, you must explicitly call `super(args)`.  
✔ If no constructor is defined in the parent class, Java provides a default constructor.

#### **Example: Calling Parameterized Parent Constructor**
```java
class Parent {
    Parent(String name) {
        System.out.println("Parent constructor: " + name);
    }
}

class Child extends Parent {
    Child() {
        super("John"); // Calls Parent's parameterized constructor
        System.out.println("Child constructor");
    }
}
```
**Output:**
```
Parent constructor: John
Child constructor
```

✔ Use `super(args)` to pass parameters to the parent constructor. 
✔ If a parent has only parameterized constructors, you must call one explicitly using `super()`.

#### Using `super()` in a Spring Boot Project

### **Answer:**
Yes, in a Spring Boot project, `super()` is commonly used when extending base classes such as controllers, services, or custom exception handlers.

#### **Example: Extending a Base Service Class**
```java
@Service
class BaseService {
    String serviceName;

    BaseService(String serviceName) {
        this.serviceName = serviceName;
        System.out.println("BaseService initialized: " + serviceName);
    }
}

@Service
class UserService extends BaseService {
    UserService() {
        super("User Service"); // Calls BaseService constructor
        System.out.println("UserService initialized");
    }
}

@SpringBootApplication
public class SpringBootApp {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(SpringBootApp.class, args);
        UserService userService = context.getBean(UserService.class);
    }
}
```

**Output:**
```
BaseService initialized: User Service
UserService initialized
```

#### **Where `super()` is Useful in Spring Boot?**
✔ **Extending `JpaRepository` or `CrudRepository`** – You don’t need `super()`, but extending a repository provides default functionality.  
✔ **Custom Exception Handling** – Calling parent exception class with `super(message)`.  
✔ **Base Controller Classes** – To share common logic across multiple controllers.

#### **Example: Using `super()` in a Custom Exception**
```java
class CustomException extends RuntimeException {
    CustomException(String message) {
        super(message);
    }
}

throw new CustomException("User not found");
```
#### What happens if you don’t explicitly call `super()` in a constructor?

If you don’t explicitly call `super()` in a constructor, the compiler automatically inserts a call to the no-argument constructor of the parent class. This means:

1. **If the parent class has a no-argument constructor**, it is called implicitly.
2. **If the parent class does not have a no-argument constructor**, a compilation error occurs.

#### **Example: When Parent Has a Default Constructor**
```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() { 
        // Compiler automatically adds super(); here
        System.out.println("Child constructor");
    }
}

public class Test {
    public static void main(String[] args) {
        new Child();
    }
}
```
**Output:**
```
Parent constructor
Child constructor
```

#### **Example: When Parent Has No Default Constructor**
```java
class Parent {
    Parent(int x) { // No default constructor
        System.out.println("Parent constructor with value: " + x);
    }
}

class Child extends Parent {
    Child() {
        // super(); // Compilation error: Parent class has no default constructor
        System.out.println("Child constructor");
    }
}
```
**Compilation Error:**
```
Constructor Parent() is undefined. Must explicitly invoke another constructor
```

#### **Key Takeaways:**
✔ If the parent class has a default constructor, the compiler calls `super()` automatically.
✔ If the parent class only has parameterized constructors, you must explicitly call `super(arguments)`. 
✔ If the parent class has no accessible constructor, the child class cannot be instantiated.

#### How does constructor chaining work in Java?

Constructor chaining in Java refers to the process of calling one constructor from another constructor within the same class or from a parent class using `this()` or `super()`. This helps in reusing code and reducing redundancy.

#### **Example: Constructor Chaining in the Same Class**
```java
class Person {
    String name;
    int age;

    Person() {
        this("Unknown", 0); // Calls the parameterized constructor
    }

    Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person();
        Person p2 = new Person("John", 25);
    }
}
```

**Output:**
```
Name: Unknown, Age: 0
Name: John, Age: 25
```

#### **Example: Constructor Chaining in Parent-Child Classes**
```java
class Parent {
    Parent() {
        System.out.println("Parent Constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // Calls the parent constructor
        System.out.println("Child Constructor");
    }
}

public class Test {
    public static void main(String[] args) {
        Child obj = new Child();
    }
}
```

**Output:**
```
Parent Constructor
Child Constructor
```

#### **Key Points:**
✔ `this()` calls another constructor within the same class.  
✔ `super()` calls the parent class constructor.  
✔ Constructor chaining ensures a smooth initialization sequence.

#### Differences Between Default, Parameterized, and Copy Constructors

#### **1. Default Constructor**
A constructor that takes no arguments and initializes an object with default values.

```java
class Person {
    String name;
    int age;

    // Default Constructor
    Person() {
        this.name = "Unknown";
        this.age = 0;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p = new Person();
        p.display();
    }
}
```
**Output:**
```
Name: Unknown, Age: 0
```

---

#### **2. Parameterized Constructor**
A constructor that accepts arguments to initialize an object with specific values.

```java
class Person {
    String name;
    int age;

    // Parameterized Constructor
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p = new Person("John", 25);
        p.display();
    }
}
```
**Output:**
```
Name: John, Age: 25
```

---

#### **3. Copy Constructor**
A constructor that creates a new object by copying values from another object of the same class.

```java
class Person {
    String name;
    int age;

    // Parameterized Constructor
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Copy Constructor
    Person(Person p) {
        this.name = p.name;
        this.age = p.age;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 30);
        Person p2 = new Person(p1); // Copy constructor
        p2.display();
    }
}
```
**Output:**
```
Name: Alice, Age: 30
```

---

#### **Key Differences**
| Type | Description | Example Usage |
|------|------------|--------------|
| **Default Constructor** | No parameters, assigns default values | Used when default initialization is needed |
| **Parameterized Constructor** | Accepts arguments to initialize fields | Used when initializing an object with specific values |
| **Copy Constructor** | Copies values from another object | Used when creating a duplicate object |

These constructors help in efficient object creation and management in Java.

#### How does Spring Boot use constructors for dependency injection?

Spring Boot supports **Constructor-based Dependency Injection**, which is preferred for mandatory dependencies as it ensures immutability and testability.

#### **Example: Constructor-based Dependency Injection**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
class ServiceA {
    void execute() {
        System.out.println("ServiceA executed");
    }
}

@Component
class ServiceB {
    private final ServiceA serviceA;

    @Autowired // Optional since Spring Boot 4.3+
    public ServiceB(ServiceA serviceA) {
        this.serviceA = serviceA;
    }

    void perform() {
        serviceA.execute();
        System.out.println("ServiceB performed");
    }
}

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class SpringBootDiExample implements CommandLineRunner {

    private final ServiceB serviceB;

    @Autowired
    public SpringBootDiExample(ServiceB serviceB) {
        this.serviceB = serviceB;
    }

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDiExample.class, args);
    }

    @Override
    public void run(String... args) {
        serviceB.perform();
    }
}
```

### **Output:**
```
ServiceA executed
ServiceB performed
```

#### **Why Constructor Injection?**
- **Ensures immutability**: Dependencies are final.
- **Better for unit testing**: No need for setters.
- **Avoids null references**: Ensures all dependencies are injected at object creation.
- **Spring Boot optimization**: Reduces boilerplate code (`@Autowired` is optional in a single constructor case).

#### **Cross-questions you might face:**
1. **Can Spring Boot inject dependencies without `@Autowired`?**
   → Yes, if there's only one constructor, Spring automatically injects dependencies.
2. **How does constructor injection compare to field injection?**
   → Constructor injection is preferred as it ensures immutability and testability.


#### Can we call a constructor explicitly from another constructor? How?
Yes, we can call a constructor explicitly from another constructor within the same class using `this()`. This is known as **constructor chaining**, which helps in reducing code duplication.

#### Constructor Chaining using `this()`**
```java
class Person {
    String name;
    int age;

    // Default constructor
    Person() {
        this("Unknown", 0); // Calls parameterized constructor
    }

    // Parameterized constructor
    Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Test {
    public static void main(String[] args) {
        Person p1 = new Person(); // Calls default constructor
        Person p2 = new Person("John", 25); // Calls parameterized constructor
    }
}
```

#### **Output:**
```
Name: Unknown, Age: 0
Name: John, Age: 25
```

#### **Why Use Constructor Chaining?**
- Reduces code duplication.
- Ensures a single point of initialization logic.
- Improves maintainability and readability.

#### **Cross-questions you might face:**
1. **Can `this()` be the last statement in a constructor?**  
   → No, `this()` must be the first statement.
2. **Can a constructor call another constructor from a different class?**  
   → No, `this()` only works within the same class. Use `super()` for calling parent class constructors.

#### What is a private constructor? How is it used in Singleton design patterns?

A **private constructor** in Java restricts object creation from outside the class. It is commonly used in **Singleton Design Patterns**, **Factory Methods**, and **Utility Classes**.

#### **Use in Singleton Design Pattern:**
The **Singleton Pattern** ensures only one instance of a class is created and provides a global access point to it.

#### **Example: Singleton using Private Constructor**
```java
class Singleton {
    private static Singleton instance;
    
    // Private constructor
    private Singleton() {
        System.out.println("Instance created");
    }
    
    // Public method to provide a single instance
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

public class Test {
    public static void main(String[] args) {
        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();
        System.out.println(obj1 == obj2); // true
    }
}
```

#### **Output:**
```
Instance created
true
```

#### **Key Advantages of Singleton with Private Constructor:**
- Ensures a **single instance** of the class.
- Saves **memory** by preventing multiple object creation.
- Useful in **database connections, logging, and thread pools**.

#### **Cross-questions you might face:**
1. **Can a private constructor be accessed in an inner class?**  
   → Yes, inner classes can access private constructors of the outer class.
2. **What happens if we try to create an instance of a class with a private constructor?**  
   → A compilation error occurs unless a static method (like `getInstance()`) is used to create an object.

#### How did you use constructors in your Microservices project (Spring Beans, Models, or Controllers)?

In a **Spring Boot Microservices project**, constructors are widely used for **dependency injection, initializing model objects, and controller instantiation**.

#### **1. Constructor-based Dependency Injection in Spring Beans**
Spring recommends **constructor-based dependency injection** to ensure immutability and better testability.

```java
@Service
public class PaymentService {
    private final PaymentRepository paymentRepository;
    
    // Constructor-based Injection
    public PaymentService(PaymentRepository paymentRepository) {
        this.paymentRepository = paymentRepository;
    }
    
    public void processPayment() {
        System.out.println("Processing payment");
    }
}
```

#### **2. Constructor in Model Classes**
Model classes often have **default, parameterized, and copy constructors** for entity creation.

```java
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String product;
    private double price;
    
    // Default Constructor
    public Order() {}
    
    // Parameterized Constructor
    public Order(String product, double price) {
        this.product = product;
        this.price = price;
    }
}
```

#### **3. Constructor in Controllers**
Spring controllers often use constructors for injecting **services**.

```java
@RestController
@RequestMapping("/orders")
public class OrderController {
    private final OrderService orderService;
    
    // Constructor Injection
    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }
    
    @GetMapping
    public List<Order> getAllOrders() {
        return orderService.getOrders();
    }
}
```

#### **Key Benefits of Using Constructors in Microservices:**
- **Enforces immutability** (fields declared `final`).
- **Easier unit testing** (no need for `@Autowired`).
- **Clear dependency visibility**.

#### **Cross-questions you might face:**
1. **Why is constructor-based injection preferred over field injection?**
   → It makes dependencies explicit, supports immutability, and is easier to test.
2. **Can we use Lombok to reduce boilerplate constructor code?**
   → Yes, `@AllArgsConstructor` and `@NoArgsConstructor` can generate constructors automatically.  


#### What is called first, constructor or init block?

In Java, the **instance initializer block (init block)** is executed **before** the constructor when an object is created. The order of execution is:
1. **Static Initializer Block** (if any, executes only once when the class is loaded)
2. **Instance Initializer Block** (executes before the constructor on every object creation)
3. **Constructor**

#### **Example:**
```java
class Example {
    // Instance initializer block
    {
        System.out.println("Instance Initializer Block executed");
    }

    // Constructor
    Example() {
        System.out.println("Constructor executed");
    }
    
    public static void main(String[] args) {
        Example obj1 = new Example();
        Example obj2 = new Example();
    }
}
```

#### **Output:**
```
Instance Initializer Block executed
Constructor executed
Instance Initializer Block executed
Constructor executed
```

#### **Explanation:**
- The instance initializer block runs **before** the constructor **each time** an object is created.
- The constructor runs **after** the instance initializer block.
- If multiple instance initializer blocks exist, they run **in the order they appear** in the class before the constructor.

#### **Cross-questions:**
1. What happens if a constructor explicitly calls another constructor using `this()`?
   → The initializer block still executes **before** any constructor.
2. Can an instance initializer block access instance variables?
   → Yes, it can initialize instance variables before the constructor runs.
3. What is the difference between static and instance initializer blocks?
   → Static blocks execute once per class loading, whereas instance blocks execute on every object creation.

#### Using Copy Constructor to Perform Deep Copy in Java

#### **What is a Copy Constructor?**
A **copy constructor** is a special type of constructor that creates a new object by copying the values of an existing object. This is particularly useful when performing a **deep copy**, ensuring that changes in the copied object do not affect the original object.

#### **Example of a Copy Constructor for Deep Copy**
```java
class Address {
    String city;
    String country;
    
    Address(String city, String country) {
        this.city = city;
        this.country = country;
    }
    
    // Copy Constructor
    Address(Address address) {
        this.city = address.city;
        this.country = address.country;
    }
}

class Employee {
    String name;
    int age;
    Address address;
    
    Employee(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
    
    // Copy Constructor for Deep Copy
    Employee(Employee emp) {
        this.name = emp.name;
        this.age = emp.age;
        this.address = new Address(emp.address); // Creating new Address object
    }
}

public class Main {
    public static void main(String[] args) {
        Address addr1 = new Address("New York", "USA");
        Employee emp1 = new Employee("John", 30, addr1);
        
        // Creating a deep copy using the copy constructor
        Employee emp2 = new Employee(emp1);
        
        // Modifying the address in the copied object
        emp2.address.city = "Los Angeles";
        
        // Checking if the original object is affected
        System.out.println(emp1.name + " lives in " + emp1.address.city); // Output: John lives in New York
        System.out.println(emp2.name + " lives in " + emp2.address.city); // Output: John lives in Los Angeles
    }
}
```

#### **Key Takeaways:**
- A copy constructor allows deep copying by creating **new instances** of mutable fields (like `Address` in the example).
- If an object contains only primitive data types, default cloning (shallow copy) may suffice.
- Using a copy constructor avoids `Cloneable` and `clone()` method complexities, making code easier to manage.
- This ensures that modifying one object does not unintentionally modify another.
---

#### Can we use `this()` and `super()` in the same constructor?

❌ **No, we cannot use both `this()` and `super()` in the same constructor.**

#### Why?
- **`this()`** is used to call another constructor **within the same class**.
- **`super()`** is used to call a constructor **from the parent class**.
- **Both must be the first statement** in a constructor, but a constructor **can have only one first statement**.

---
#### ❌ Invalid Example (Compilation Error)
```java
class Parent {
    Parent() {
        System.out.println("Parent Constructor");
    }
}

class Child extends Parent {
    Child() {
        this();  // ❌ Calls another constructor in the same class
        super(); // ❌ Calls parent constructor (Error: must be first)
        System.out.println("Child Constructor");
    }
}
```
#### 🚨 Compilation Error:
> Constructor call must be the first statement in a constructor.

---
#### ✅ Correct Approach (Using Constructor Chaining)
You **can** use both `this()` and `super()` **indirectly** by chaining constructors:

```java
class Parent {
    Parent() {
        System.out.println("Parent Constructor");
    }
}

class Child extends Parent {
    Child() {
        super(); // ✅ Calls Parent constructor
        System.out.println("Child Constructor");
    }

    Child(int x) {
        this();  // ✅ Calls the default constructor of Child
        System.out.println("Child Constructor with parameter: " + x);
    }
}

public class Main {
    public static void main(String[] args) {
        new Child(10);
    }
}
```

#### ✅ Expected Output:
```
Parent Constructor
Child Constructor
Child Constructor with parameter: 10
```
---