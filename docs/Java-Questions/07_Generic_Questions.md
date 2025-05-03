
#### **Explain System.out.println() statement**

**Answer:**
- `System` is a class in the `java.lang` package.
- `out` is a static member of the `System` class and an instance of `java.io.PrintStream`.
- `println()` is a method of the `PrintStream` class, which is used to print messages to the console with a newline.

#### **Example:**

```java
public class PrintlnExample {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
        System.out.println(100);
        System.out.println(3.14);
        System.out.println(true);
    }
}
```

#### **Output:**
```
Hello, World!
100
3.14
true
```

---

#### Explain Auto-boxing and Un-boxing**

**Answer:**
In Java 1.5, Auto-boxing and Un-boxing were introduced to automatically convert primitive types to their corresponding wrapper classes and vice versa.

#### **Key Points:**
- **Auto-boxing:** Conversion of a primitive type into its corresponding wrapper class.
- **Un-boxing:** Conversion of a wrapper class object back into a primitive type.

#### **Example:**

```java
public class BoxingExample {
    public static void main(String[] args) {
        // Auto-boxing: int to Integer
        int num = 10;
        Integer boxedNum = num;
        System.out.println("Auto-boxing: " + boxedNum);
        
        // Un-boxing: Integer to int
        Integer obj = new Integer(20);
        int unboxedNum = obj;
        System.out.println("Un-boxing: " + unboxedNum);
    }
}
```

#### **Output:**
```
Auto-boxing: 10
Un-boxing: 20
```

#### Explain static keyword in Java?

**Answer:** In Java, a `static` member is a member of a class that isn’t associated with an instance of a class. Instead, the member belongs to the class itself.

#### Static is applicable for:
- **Variable**
- **Method**
- **Block**
- **Nested class**

#### **Static Variable:**
- If a variable is declared as `static`, it is known as a *static variable*.
- **Only one copy** of the variable is created and shared among all instances of the class.
- The static variable gets memory **only once** in the class area when the class is loaded.
- **Use case:** Declare common properties for all objects, e.g., company name of employees.

#### **Static Method:**
- A method declared with the `static` keyword belongs to the class rather than to any object.
- **Access directly** using the class name without creating an object.
- **Rules:**
  - Cannot access **non-static** methods or variables.
  - Cannot use `this` or `super` inside a static method.
- **Example:** The `main()` method is static, allowing Java to start an application without creating an object.

#### **Static Block:**
- Executed **once** when the class is loaded.
- Used to initialize **static variables**.

#### **Static Nested Classes:**
- A special type of **inner class** where the inner class is static.
- Can **only access static members** of the outer class.
- **Advantage:** Improves code readability and maintainability.
- Unlike normal inner classes, **a static nested class can exist without an instance of the outer class**.

#### **How to create an object of a static inner class?**
```java
OuterClass.StaticNestedClass nestedClassObject = new OuterClass.StaticNestedClass();
```

#### **Error Scenario:**
- **Compile-time error occurs** when trying to access a **non-static** member inside a static nested class.

---

#### **Example: Using Inner Class Object**
```java
class OuterClass {
    static class StaticNestedClass {
        void display() {
            System.out.println("Static Nested Class Method");
        }
    }
    public static void main(String[] args) {
        OuterClass.StaticNestedClass obj = new OuterClass.StaticNestedClass();
        obj.display();
    }
}
```
**Output:**
```
Static Nested Class Method
```

#### **Example: Static Members in Static Inner Class**
```java
class OuterClass {
    static class StaticNestedClass {
        static void staticMethod() {
            System.out.println("Static method in static nested class");
        }
    }
    public static void main(String[] args) {
        OuterClass.StaticNestedClass.staticMethod(); // No need to create an object
    }
}
```
**Output:**
```
Static method in static nested class
```

#### What is an Inner Class in Java, how it can be instantiated, and what are the types of Inner Classes?

**Answer:** In Java, when you define one **non-static** class within another class, it is called an **Inner Class (Nested Class)**. Inner classes allow logically grouping classes that are only used in one place, thereby increasing encapsulation and making the code more readable and maintainable.

#### **Key Points about Inner Classes:**
- An **inner class is associated with the object** of the outer class and can access all variables and methods of the outer class.
- **Static variables and static methods are not allowed** in non-static inner classes since they are associated with instances.
- To create an **instance of an inner class**, an instance of the outer class is required first.

#### **Types of Inner Classes:**
1. **Member Inner Class** (Regular Inner Class)
2. **Static Nested Class**
3. **Method-local Inner Class**
4. **Anonymous Inner Class**

#### **How to instantiate an Inner Class?**
```java
OuterClass outerObject = new OuterClass();
OuterClass.InnerClass innerObject = outerObject.new InnerClass();
```

---

#### **Example: Member Inner Class**
```java
class OuterClass {
    private String message = "Hello from Outer Class";
    
    class InnerClass {
        void display() {
            System.out.println(message);
        }
    }
    
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display();
    }
}
```
**Output:**
```
Hello from Outer Class
```

#### **Example: Compile-time error when static variable/method is present in Inner Class**
```java
class OuterClass {
    class InnerClass {
        static int data = 100; // Compile-time error
        static void display() { // Compile-time error
            System.out.println("Static method inside inner class");
        }
    }
}
```
**Error:**
```
Inner classes cannot have static members.
```

---

#### **Special Types of Inner Classes:**
#### **1. Local Inner Class:**
- Defined **inside a block**, usually within a method, loop, or if clause.
- **Not a member of the enclosing class** but belongs to the block it is defined in.
- Cannot have **access modifiers**, but can be `final` or `abstract`.
- **Has access to the enclosing class's members.**
- Must be **instantiated within the block** it is defined in.

#### **Points to remember:**
- Cannot be instantiated **outside** the block where they are defined.
- Has access to **members** of the enclosing class.
- **Until Java 1.7:** Can only access `final` local variables of the enclosing block.
- **From Java 1.8 onwards:** Can access **non-final** local variables of the enclosing block.
- **Scope is restricted** to the block where they are defined.
- Can **extend an abstract class** or **implement an interface**.

#### **Example: Local Inner Class**
```java
class OuterClass {
    void outerMethod() {
        class LocalInner {
            void display() {
                System.out.println("This is a Local Inner Class");
            }
        }
        LocalInner localInner = new LocalInner();
        localInner.display();
    }
    
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.outerMethod();
    }
}
```
**Output:**
```
This is a Local Inner Class
```

#### **Compile-time Error Example: Modifying Block-Level Variables**
```java
class OuterClass {
    void outerMethod() {
        int x = 10; // Effectively final
        class LocalInner {
            void display() {
                System.out.println("Value: " + x);
            }
        }
        LocalInner obj = new LocalInner();
        obj.display();
        x = 20; // Compile-time error
    }
}
```

---

#### **2. Anonymous Inner Class:**
- A **class without a name**.
- Used when you need a class only **once**.
- **Cannot have a constructor** since it has no class name.
- Cannot be declared as **static**.
- Generally used to **override methods** of a class or interface.

#### **Use Case Example:** Sorting Employees using an Anonymous Inner Class
```java
import java.util.*;

class Employee {
    String name;
    int age;
    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String toString() {
        return name + " - " + age;
    }
}

public class AnonymousInnerDemo {
    public static void main(String[] args) {
        List<Employee> list = new ArrayList<>();
        list.add(new Employee("John", 30));
        list.add(new Employee("Alice", 25));
        list.add(new Employee("Bob", 28));
        
        Collections.sort(list, new Comparator<Employee>() {
            public int compare(Employee e1, Employee e2) {
                return e1.name.compareTo(e2.name);
            }
        });
        
        System.out.println(list);
    }
}
```
**Output:**
```
[Alice - 25, Bob - 28, John - 30]
```

#### **Example: Using Anonymous Inner Class for Runnable**
```java
class Test {
    public static void main(String[] args) {
        Runnable r = new Runnable() {
            public void run() {
                System.out.println("Running thread using anonymous inner class");
            }
        };
        Thread t = new Thread(r);
        t.start();
    }
}
```
**Output:**
```
Running thread using anonymous inner class
```

#### **Example: Overriding a Parent Class Method**
```java
class Parent {
    void show() {
        System.out.println("Parent class method");
    }
}
class Test {
    public static void main(String[] args) {
        Parent obj = new Parent() {
            void show() {
                System.out.println("Anonymous Inner Class Method");
            }
        };
        obj.show();
    }
}
```
**Output:**
```
Anonymous Inner Class Method
```

#### **Compile-time Error: Using Non-Final Static Variable in Anonymous Class**
```java
class Test {
    static int x = 10;
    public static void main(String[] args) {
        new Object() {
            void display() {
                // x++; // Compile-time error: Cannot modify non-final static variable
                System.out.println(x);
            }
        };
    }
}
```

#### **What is Variable Shadowing and Variable Hiding in Java?**

#### **1. Variable Shadowing**
Variable shadowing occurs when a **local variable** inside a method has the **same name** as an instance variable. The local variable **shadows** the instance variable inside the method block.

#### **Example of Variable Shadowing:**
```java
class ShadowExample {
    int x = 10; // Instance variable

    void display() {
        int x = 20; // Local variable shadows instance variable
        System.out.println("Local x: " + x); // Prints 20
        System.out.println("Instance x: " + this.x); // Prints 10
    }

    public static void main(String[] args) {
        ShadowExample obj = new ShadowExample();
        obj.display();
    }
}
```
**Output:**
```
Local x: 20
Instance x: 10
```

If you want to access the instance variable, you can do so using the **`this`** keyword.

---

#### **2. Variable Hiding**
Variable hiding occurs when **both the parent and child classes** have a variable with the **same name**. The child class variable hides the parent class variable.

#### **Example of Variable Hiding:**
```java
class Parent {
    int x = 10;
}

class Child extends Parent {
    int x = 20; // Hides Parent's x
    
    void show() {
        System.out.println("Child x: " + x); // Prints 20
        System.out.println("Parent x: " + super.x); // Prints 10
    }
    
    public static void main(String[] args) {
        Child obj = new Child();
        obj.show();
    }
}
```

**Output:**
```
Child x: 20
Parent x: 10
```

If you want to access the parent class variable, you can do this using the **`super`** keyword.

---

#### **What is the Difference Between Variable Hiding and Method Overriding**
- **Variable hiding** is **not** the same as **method overriding**.
- In **method overriding**, the overridden method **replaces** the inherited method.
- In **variable hiding**, the child class **hides** the inherited variable instead of replacing it.

#### **Example to Demonstrate the Difference:**
```java
class Parent {
    int x = 10;
    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    int x = 20; // Variable Hiding
    
    @Override
    void display() {
        System.out.println("Child method"); // Method Overriding
    }
    
    public static void main(String[] args) {
        Parent obj = new Child();
        obj.display(); // Calls Child's method (Overriding)
        System.out.println("x: " + obj.x); // Prints Parent's x (Variable Hiding)
    }
}
```

**Output:**
```
Child method
x: 10
```

---

#### **Key Differences:**
| Feature             | Variable Hiding | Method Overriding |
|---------------------|----------------|-------------------|
| Applies To         | Variables       | Methods          |
| Resolution Based On | Reference Type  | Object Type      |
| Inheritance        | Yes             | Yes              |
| Behavior           | Parent variable is hidden, not replaced | Parent method is replaced |

#### Explain enum in Java**

An **enum** in Java is a special data type that contains a **fixed set of constants**. Enums improve **type safety** and can also have **fields, methods, and constructors**.

#### **Key Points about Enums:**
- Enum constants are **static** and **final** implicitly.
- Enum **improves type safety** by ensuring only predefined values are used.
- Enum can be **declared inside or outside of a class**.
- Enum can have **fields, constructors (private), and methods**.
- Enum **cannot extend** any class because it **implicitly extends** `java.lang.Enum` but can implement interfaces.
- Enum can be used in **switch statements**.
- Enum can have a **main() method** inside it.
- Enum provides built-in methods:
  - **`values()`** → Returns an array of all enum constants.
  - **`ordinal()`** → Returns the index of an enum constant.
  - **`valueOf(String name)`** → Returns the enum constant for a given name.
- Enum **can be traversed** using a loop.
- Enum **can have abstract methods**.
- Enum **cannot be instantiated** because its constructor is **private**.
- Enum constructors are executed **when the enum class is loaded**.
- Enum constants **must be declared first** before any fields or methods.

---

#### **Example 1: Basic Enum Usage**
```java
enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY;
}

public class EnumExample {
    public static void main(String[] args) {
        Day today = Day.SATURDAY;
        System.out.println("Today is: " + today);
    }
}
```
**Output:**
```
Today is: SATURDAY
```

---

#### **Example 2: Enum with Fields, Methods, and Constructor**
```java
enum Color {
    RED("#FF0000"), GREEN("#00FF00"), BLUE("#0000FF");
    
    private String hexCode;
    
    // Private constructor
    private Color(String hexCode) {
        this.hexCode = hexCode;
    }
    
    public String getHexCode() {
        return hexCode;
    }
}

public class EnumExample2 {
    public static void main(String[] args) {
        Color c = Color.RED;
        System.out.println("Color: " + c + ", Hex Code: " + c.getHexCode());
    }
}
```
**Output:**
```
Color: RED, Hex Code: #FF0000
```

---

#### **Example 3: Using Enum in a Switch Statement**
```java
enum Level {
    LOW, MEDIUM, HIGH;
}

public class EnumSwitchExample {
    public static void main(String[] args) {
        Level level = Level.HIGH;

        switch (level) {
            case LOW:
                System.out.println("Low level");
                break;
            case MEDIUM:
                System.out.println("Medium level");
                break;
            case HIGH:
                System.out.println("High level");
                break;
        }
    }
}
```
**Output:**
```
High level
```

---

#### **Example 4: Using `values()`, `ordinal()`, and `valueOf()` Methods**
```java
enum Size {
    SMALL, MEDIUM, LARGE;
}

public class EnumMethodsExample {
    public static void main(String[] args) {
        // Using values()
        for (Size s : Size.values()) {
            System.out.println(s + " at index " + s.ordinal());
        }
        
        // Using valueOf()
        Size size = Size.valueOf("MEDIUM");
        System.out.println("Selected Size: " + size);
    }
}
```
**Output:**
```
SMALL at index 0
MEDIUM at index 1
LARGE at index 2
Selected Size: MEDIUM
```

---

#### **Example 5: Enum Implementing an Interface**
```java
interface Printable {
    void print();
}

enum Status implements Printable {
    SUCCESS, FAILURE;
    
    @Override
    public void print() {
        System.out.println("Status: " + this);
    }
}

public class EnumInterfaceExample {
    public static void main(String[] args) {
        Status.SUCCESS.print();
        Status.FAILURE.print();
    }
}
```
**Output:**
```
Status: SUCCESS
Status: FAILURE
```

#### What is Cloneable?

`Cloneable` is an interface in Java that must be implemented by a class to allow its objects to be cloned.

- A class implements the `Cloneable` interface to indicate to the `Object.clone()` method that it is legal to make a field-for-field copy of instances of that class.
- If you try to clone an object that does not implement the `Cloneable` interface, it will throw `CloneNotSupportedException`.

#### **Example Without Implementing Cloneable Interface:**
```java
class Employee {
    int id;
    String name;
    
    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

public class TestClone {
    public static void main(String[] args) {
        Employee e1 = new Employee(101, "John");
        Employee e2 = e1; // e2 now refers to the same object as e1
        
        e2.name = "Doe";
        System.out.println(e1.name); // Output: Doe (Changes reflect in e1 as well)
    }
}
```
**Explanation:** Here, `e2` is assigned the reference of `e1`, meaning any changes made to `e2` will also affect `e1`.

---

#### **Implementing Cloneable Interface:**
```java
class Employee implements Cloneable {
    int id;
    String name;
    
    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // Overriding clone method
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class TestClone {
    public static void main(String[] args) throws CloneNotSupportedException {
        Employee e1 = new Employee(101, "John");
        Employee e2 = (Employee) e1.clone(); // Cloning e1
        
        e2.name = "Doe";
        System.out.println(e1.name); // Output: John (Original object remains unchanged)
    }
}
```
**Explanation:** Since `clone()` is used, `e2` is a separate object, and changes made to `e2` do not affect `e1`.

---

#### **Shallow Copy vs Deep Copy:**
If a class contains object references (other than primitive types), `Object.clone()` performs a **shallow copy**, meaning it copies the references instead of creating new objects.

**Example with Shallow Copy:**
```java
class Address {
    String city;
    Address(String city) {
        this.city = city;
    }
}

class Employee implements Cloneable {
    int id;
    String name;
    Address address;
    
    Employee(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }
    
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class TestClone {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("New York");
        Employee e1 = new Employee(101, "John", address);
        Employee e2 = (Employee) e1.clone();
        
        e2.address.city = "Los Angeles";
        System.out.println(e1.address.city); // Output: Los Angeles (Shallow Copy Issue)
    }
}
```
**Explanation:** Since `clone()` only copies references, modifying `e2.address.city` also affects `e1`.

To achieve **Deep Copy**, manually clone referenced objects inside the `clone()` method.

**Example with Deep Copy:**
```java
class Address implements Cloneable {
    String city;
    Address(String city) {
        this.city = city;
    }
    
    protected Object clone() throws CloneNotSupportedException {
        return new Address(this.city);
    }
}

class Employee implements Cloneable {
    int id;
    String name;
    Address address;
    
    Employee(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }
    
    protected Object clone() throws CloneNotSupportedException {
        Employee cloned = (Employee) super.clone();
        cloned.address = (Address) this.address.clone(); // Manually cloning reference type
        return cloned;
    }
}

public class TestClone {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("New York");
        Employee e1 = new Employee(101, "John", address);
        Employee e2 = (Employee) e1.clone();
        
        e2.address.city = "Los Angeles";
        System.out.println(e1.address.city); // Output: New York (Deep Copy Achieved)
    }
}
```

#### **Key Differences Between Shallow and Deep Copy:**
| Feature          | Shallow Copy | Deep Copy |
|-----------------|-------------|-----------|
| Copy Type       | Copy of reference | Copy of actual object |
| Affects Original? | Yes | No |
| Complexity      | Simple | More complex (requires manual cloning) |

---

#### **Cross-questions You Might Face:**
1. **What happens if a class does not implement `Cloneable` but calls `clone()`?**  
   → It throws `CloneNotSupportedException`.
2. **How can you achieve cloning if a class does not implement `Cloneable`?**  
   → Use **copy constructors** or serialization/deserialization.
3. **Is `clone()` a deep copy by default?**  
   → No, Java's `Object.clone()` performs a **shallow copy**.
4. **Which method should be overridden to perform deep copying?**  
   → The `clone()` method should be overridden to manually clone reference variables.

---

This document explains `Cloneable` with examples, covering **shallow copy vs deep copy**. Let me know if you need further clarifications!

#### Deep Copy in Java

Deep copying in Java ensures that a cloned object has a completely independent copy of its fields, including any referenced objects. Here, we discuss two methods to perform deep copying:

#### 1. Using Serialization
This approach serializes the original object into a byte stream and then deserializes it to create a deep copy.

#### Example:
```java
import java.io.*;

class Address implements Serializable {
    String city;

    Address(String city) {
        this.city = city;
    }
}

class Person implements Serializable {
    String name;
    Address address;

    Person(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    public Person deepCopy() throws IOException, ClassNotFoundException {
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(this);
        oos.flush();
        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bis);
        return (Person) ois.readObject();
    }
}

public class DeepCopyExample {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Address address = new Address("New York");
        Person p1 = new Person("John", address);
        Person p2 = p1.deepCopy();

        p2.address.city = "Los Angeles";

        System.out.println(p1.address.city); // Output: New York
        System.out.println(p2.address.city); // Output: Los Angeles
    }
}
```

#### 2. Using Apache Commons Lang (`SerializationUtils.clone()`)
Apache Commons Lang provides an easier way to perform deep copying via `SerializationUtils.clone()`. All involved classes must implement `Serializable`.

#### Example:
```java
import org.apache.commons.lang3.SerializationUtils;
import java.io.Serializable;

class Address implements Serializable {
    String city;

    Address(String city) {
        this.city = city;
    }
}

class Person implements Serializable {
    String name;
    Address address;

    Person(String name, Address address) {
        this.name = name;
        this.address = address;
    }
}

public class DeepCopyApacheExample {
    public static void main(String[] args) {
        Address address = new Address("New York");
        Person p1 = new Person("John", address);
        Person p2 = SerializationUtils.clone(p1);

        p2.address.city = "Los Angeles";

        System.out.println(p1.address.city); // Output: New York
        System.out.println(p2.address.city); // Output: Los Angeles
    }
}
```

#### Maven Dependency:
To use Apache Commons Lang, add this dependency in your `pom.xml`:
```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.12.0</version>
</dependency>
```

#### Conclusion
Both methods provide a deep copy, ensuring that changes in the copied object do not affect the original.
- **Serialization** is built-in but requires manual handling.
- **Apache Commons Lang** simplifies deep copying but requires an external dependency.