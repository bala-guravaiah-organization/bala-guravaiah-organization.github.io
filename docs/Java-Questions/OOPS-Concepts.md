#### What are the 4 pillars of OOPS?
1. Abstraction
2. Encapsulation
3. Inheritance
4. Polymorphism

---

#### 1. Abstraction
Abstraction is the process of hiding the implementation details and showing only functionality to the user.

#### Real-world examples:
- **TV Remote:** To start the TV, you press the power button. You don’t need to know about the internal circuit operations like how infrared waves are passing.
- **Car Gears:** We know what happens when we change the gear, but we don’t know how the mechanism works internally. That information is abstracted from us.

#### In Java, Abstraction can be achieved in two ways:
- **Abstract classes**
- **Interfaces**

---

#### 2. Encapsulation
Encapsulation is the process of binding data and methods within a class. It ensures that essential details of a class are controlled using access modifiers (**public, private, protected**). This leads to the desired level of Abstraction.

#### Example:
A **Java Bean**, where all data members are made `private` and public methods are provided to access them.

---

#### 3. Inheritance
Inheritance defines a **parent-child relationship** between classes, enabling code reuse. The child class inherits properties and behaviors from the parent class.

#### Key Points:
- **Code reusability** is the biggest advantage of Inheritance.
- Java does not support **multiple inheritance** through classes but allows it through **interfaces**.

---

#### 4. Polymorphism
Polymorphism means "many forms." It allows an object or function to take different forms.

#### Types of Polymorphism:
1. **Compile-Time Polymorphism (Method Overloading)**
   - Method overloading occurs when two or more methods in a class have the **same method name** but **different arguments**.
   - The method to be called is decided at **compile-time**.

2. **Run-Time Polymorphism (Method Overriding)**
   - Overriding happens when a **child class** provides a specific implementation for a method that is already defined in the **parent class**.
   - The method call is resolved at **runtime**.

---

#### What is an abstract class?
A class that is declared using the `abstract` keyword is known as an **abstract class**. It can have both **abstract methods** (methods without a body) and **concrete methods** (methods with a body).

#### Key Points to Remember:
- An **abstract class cannot be instantiated**, meaning you cannot create an object of an abstract class.
  - This also means an abstract class has no use unless it is **extended** by some other class.
- If there is any **abstract method** in a class, then that class **must be declared abstract**.
- The first **non-abstract class** that extends an abstract class **must provide implementations** for all abstract methods defined in the abstract class.

#### Example Code:
```java
package com.tech;

abstract class MyAbstractClass {
    // Abstract method
    abstract void print();
    
    // Concrete method
    public void display() {
        System.out.println("In display method");
    }
}

public class AbstractDemo extends MyAbstractClass {

    @Override
    void print() {
        System.out.println("In print method");
    }

    public static void main(String[] args) {
        AbstractDemo obj = new AbstractDemo();
        obj.print();
        obj.display();
    }
}
```
#### Output:
```
In print method  
In display method  
```

---

#### Does an Abstract class have a constructor?
Yes, abstract classes have constructors. You can either provide one explicitly or Java will provide a default constructor.

#### Why do abstract classes need constructors?
Although you cannot create an object of an abstract class, **constructors are used to initialize data members** when a child class extends the abstract class.

When an abstract class is extended, its constructor is invoked when an object of the subclass is created. The first line of a subclass constructor always calls the **superclass constructor**, ensuring proper initialization.

#### Example Code:
```java
package com.tech;

// Abstract class with parameterized constructor
abstract class MyAbstractClass {
    public int a;
    public int b;

    public MyAbstractClass(int a, int b) {
        this.a = a;
        this.b = b;
    }

    public void print() {
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}

// Concrete subclass
public class AbstractDemo extends MyAbstractClass {
    
    public AbstractDemo(int x, int y) {
        super(x, y);  // Explicit call to superclass constructor
    }

    public static void main(String[] args) {
        AbstractDemo obj = new AbstractDemo(5, 10);
        obj.print();
    }
}
```
#### Output:
```
a = 5  
b = 10  
```

---

#### what are the Differences between Abstract Class and Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Methods | Can have both abstract and concrete methods | Can only have abstract methods (except Java 8+ which allows default & static methods) |
| Access Modifiers | Can have any access modifier | Methods are implicitly public and abstract |
| Variables | Can have final, non-final, static, and non-static variables | Variables are always static and final |
| Inheritance | A class can extend only one abstract class | A class can implement multiple interfaces |
| Extending & Implementing | Can extend another class and implement multiple interfaces | Can only extend other interfaces |

---

#### What to choose – Interface or Abstract Class?

- Use **abstract class** when you want to provide **default implementations** of methods that subclasses can directly use.
- Use **interfaces** when your **contract keeps changing** to avoid forcing changes on implementing classes.
- **Best practice:** Prefer **interfaces** in most cases.

---

#### Why was Java 8 introduced default methods?

Default methods were introduced in Java 8 to allow adding new methods to interfaces **without breaking existing implementations**.

#### Example Scenario:
- If 100 classes implement an interface and a new method is added, all 100 classes would need modification.
- With **default methods**, the new method can have a default implementation, avoiding the need for updates in all classes.

#### Diamond Problem:
If two interfaces define the same **default method**, a class implementing both must override the method to resolve ambiguity.

#### How does Java handle the Diamond Problem with default methods?

When two interfaces have default methods with the same name, and a class implements both interfaces **without overriding the method**, Java will throw a compilation error due to ambiguity. To resolve this, the implementing class must **explicitly override the conflicting method** and specify which interface's method to call.

#### Example Code:
```java
interface Interface1 {
    default void hello() {
        System.out.println("Hello from Interface1");
    }
}

interface Interface2 {
    default void hello() {
        System.out.println("Hello from Interface2");
    }
}

public class Child implements Interface1, Interface2 {
    
    @Override
    public void hello() {
        System.out.println("inside Child class hello method");
        Interface1.super.hello();  // Explicitly call Interface1's implementation
    }

    public static void main(String[] args) {
        Child obj = new Child();
        obj.hello();
    }
}
```

#### Output:
```
inside Child class hello method  
Hello from Interface1  
```

#### Explanation:
- **`Interface1` and `Interface2`** both define a default method `hello()`.
- **`Child` class implements both interfaces**, leading to a potential conflict.
- **To resolve the ambiguity**, `Child` class **overrides `hello()`** and explicitly calls `Interface1.super.hello()` to specify which implementation to use.

By overriding the method, the Diamond Problem is resolved in Java.

#### Why Java 8 has introduced static methods?

Before Java 8, utility methods were typically placed in classes with static methods, such as `Collections` or `Math`. However, Java 8 introduced **static methods in interfaces** to allow utility methods to be defined directly within interfaces.

#### Benefits of Static Methods in Interfaces:
- **Better Organization:** Utility methods related to an interface can be directly placed inside the interface rather than an unrelated helper class.
- **Performance Optimization:** Using an interface with static methods is more efficient than creating a separate utility class.
- **No Need for Implementation:** Unlike default methods, static methods do not require implementation in implementing classes.

#### Example Code:
```java
interface UtilityInterface {
    static void show() {
        System.out.println("Static method in Interface");
    }
}

public class StaticMethodDemo {
    public static void main(String[] args) {
        UtilityInterface.show(); // Calling static method from interface
    }
}
```

#### Output:
```
Static method in Interface  
```

This allows defining utility methods directly in interfaces without requiring an implementing class, improving modularity and design flexibility.

#### Why Java does not allow multiple inheritance?

Multiple inheritance occurs when a class has more than one parent class.

#### Why Java does not allow this?
Let us consider there are two parent classes having a method named `hello()` with the same signature, and one child class extends these two classes. If you call this `hello()` method, which is present in both parents, it results in ambiguity—this is called the **Diamond Problem**.

If you try to extend more than one class in Java, you will get a **compile-time error**.

---

#### Example Code:

```java
class Parent1 {
    public void hello() {
        System.out.println("Hello from Parent1 class");
    }
}

class Parent2 {
    public void hello() {
        System.out.println("Hello from Parent2 class");
    }
}

// Java does NOT support multiple class inheritance
public class Child extends Parent1 /* Syntax error here: cannot extend Parent2 */ {
    // Error: Java only allows single class inheritance
}
```

#### Explanation:
- Since both `Parent1` and `Parent2` have a method with the same signature, Java does not allow the `Child` class to extend both.
- This restriction prevents ambiguity and makes Java **more maintainable and predictable**.

---

#### What are the rules for Method Overloading and Method Overriding?

#### Method Overloading Rules:
Two methods can be called overloaded if they follow the rules below:
- **Both must have the same method name.**
- **Both must have different arguments.**

If both methods follow the above two rules, then they **may or may not**:
- Have different access modifiers.
- Have different return types.
- Throw different checked or unchecked exceptions.

---

#### Method Overriding Rules:
The overriding method of a child class must follow the rules below:
- It must have the **same method name** as that of the parent class method.
- It must have the **same arguments** as that of the parent class method.
- It must have **either the same return type or a covariant return type** (child classes are covariant types to their parents).
- It must not throw **broader checked exceptions** than the parent method.
- It must not have a **more restrictive access modifier** (if the parent method is `public`, then the child method **cannot** be `private` or `protected`).

#### Can we override final methods?

No, final methods cannot be overridden.

---

#### Can constructors and private methods be overridden?

No.

---

#### What is the final keyword and where can it be used?

- If you use `final` with a **primitive type variable**, then its value cannot be changed once assigned.
- If you use `final` with a **method**, then you cannot override it in the subclass.
- If you use `final` with a **class**, then that class cannot be extended.
- If you use `final` with an **object type**, then that object cannot be referenced again.

#### What is a Marker Interface?

A **Marker Interface** is an interface that does not contain any methods or fields. Some common marker interfaces in Java include:
- `Cloneable`
- `Serializable`
- `Remote`

It is a common misconception that marker interfaces signal something to the JVM or compiler; in reality, they do not. Instead, they serve as a way to check for certain behaviors using the `instanceof` operator.

#### Example: Cloneable Interface
In Java, you cannot call the `clone()` method on an object unless the class implements the `Cloneable` interface. This is enforced using:
```java
if (!(obj instanceof Cloneable)) {
    throw new CloneNotSupportedException();
}
```
Similarly, for serialization, `ObjectOutputStream` checks if an object implements `Serializable` before writing it:
```java
if (!(obj instanceof Serializable)) {
    throw new NotSerializableException();
}
```

#### Can you write your own custom Marker Interface?

Yes! Since marker interfaces are just used for classification and checked using `instanceof`, you can define your own.

#### Example:
```java
public interface MyMarkerInterface {
}

public class MyClass implements MyMarkerInterface {
}

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        if (obj instanceof MyMarkerInterface) {
            System.out.println("Object is an instance of MyMarkerInterface");
        }
    }
}
```
#### Ambiguity method overloading in java

```java
public class Test {
    public void print(int a, long b) {
        System.out.println("Method 1");
    }

    public void print(long a, int b) {
        System.out.println("Method 2");
    }

    public static void main(String[] args) {
        Test obj = new Test();
        obj.print(5, 10); // Compilation Error
    }
}
```

#### Explanation:
The above code results in a **compilation error** because the method call `obj.print(5, 10);` is ambiguous. Here’s why:

- The first method `print(int a, long b)` expects an `int` as the first argument and a `long` as the second argument.
- The second method `print(long a, int b)` expects a `long` as the first argument and an `int` as the second argument.
- The provided arguments `5` and `10` are both of type `int`. Java’s compiler gets confused because:
  - It can promote `5` to `long` for the second method.
  - It can promote `10` to `long` for the first method.

Since Java does not have a clear rule to resolve this ambiguity, the compilation fails with an **error: The method print(int, long) is ambiguous for the type Test**.

#### Fix:
To avoid ambiguity, you can explicitly cast one of the arguments:

```java
obj.print(5L, 10); // Calls Method 2
obj.print(5, 10L); // Calls Method 1
```

---

#### Method Overriding Example:

```java
class Parent {
    public void print(int a, long b) {
        System.out.println("Parent: Method 1 (int, long)");
    }

    public void print(long a, int b) {
        System.out.println("Parent: Method 2 (long, int)");
    }
}

class Child extends Parent {
    @Override
    public void print(int a, long b) {
        System.out.println("Child: Overridden Method 1 (int, long)");
    }
}

public class OverridingExample {
    public static void main(String[] args) {
        Parent obj1 = new Parent();
        obj1.print(5, 10L); // Calls Parent: Method 1
        obj1.print(5L, 10); // Calls Parent: Method 2

        Parent obj2 = new Child();
        obj2.print(5, 10L); // Calls Child: Overridden Method 1
        obj2.print(5L, 10); // Calls Parent: Method 2
    }
}
```

#### Output:
```
Parent: Method 1 (int, long)
Parent: Method 2 (long, int)
Child: Overridden Method 1 (int, long)
Parent: Method 2 (long, int)
```

#### Explanation:
- If a method is overridden in a subclass, the overridden version is called when using a reference of the parent class pointing to a child object.
- `print(int, long)` is overridden, so the child class's method is executed.
- `print(long, int)` is not overridden, so the parent class's method is used even when the reference is of type `Parent` but pointing to a `Child` object.
---

#### What are the differences between Composition and Aggregation ?

| Feature           | **Composition (Strong Association)** | **Aggregation (Weak Association)** |
|-------------------|--------------------------------------|--------------------------------------|
| **Relationship Type** | Strong Association | Weak Association |
| **Dependency** | Child **cannot exist** without the parent. | Child **can exist** without the parent. |
| **Lifespan** | If the parent is deleted, the child is also deleted. | If the parent is deleted, the child can still exist. |
| **Example** | Car and Engine | Student and College |

#### **Code Examples**

#### **Composition Example: Car and Engine**
In **composition**, the child object is created inside the parent and cannot exist independently.

```java  
class Engine {  
    void start() {  
        System.out.println("Engine started...");  
    }  
}  

class Car {  
    private final Engine engine;  // Composition: Engine is part of Car  

    public Car() {  
        this.engine = new Engine();  // Engine is created within Car  
    }  

    void drive() {  
        engine.start();  
        System.out.println("Car is moving...");  
    }  
}  

public class Main {  
    public static void main(String[] args) {  
        Car car = new Car();  
        car.drive();  
        // If Car is deleted, Engine is also deleted  
    }  
}  
```

#### **Aggregation Example: Student and College**
In **aggregation**, the child object exists separately and is passed as a reference to the parent.

```java  
class College {  
    String name;  

    College(String name) {  
        this.name = name;  
    }  
}  

class Student {  
    String studentName;  
    College college;  // Aggregation: Student has a reference to College  

    Student(String studentName, College college) {  
        this.studentName = studentName;  
        this.college = college;  
    }  

    void display() {  
        System.out.println(studentName + " studies at " + college.name);  
    }  
}  

public class Main {  
    public static void main(String[] args) {  
        College college = new College("ABC University"); // College exists independently  
        Student student = new Student("John", college);  
        student.display();  
        // If Student is deleted, College still exists  
    }  
}  
```

#### **Key Differences in Code**
1. **Composition**: The `Engine` object is **created inside** the `Car` class and **cannot exist separately**.
2. **Aggregation**: The `College` object **exists separately** and is **passed as a reference** to `Student`.

#### **Summary**
- **Composition**: Strong association where the child **cannot exist** without the parent.
- **Aggregation**: Weak association where the child **can exist independently** of the parent.

---