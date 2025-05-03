
#### Why is String Immutable?

String is immutable in Java for the following reasons:

1. **String Pool**: 
   - Java maintains a special memory area called the **String Pool** in the heap.
   - When a new String is created, if an identical String already exists in the pool, the existing reference is returned instead of creating a new object.
   - If Strings were mutable, modifying one reference would affect all other references, leading to incorrect behavior.

2. **Security**: 
   - Strings are used in security-sensitive areas like **network connections, database URLs, usernames, and passwords**.
   - If Strings were mutable, an attacker could alter these values, causing security vulnerabilities.

3. **Multithreading**: 
   - Since Strings are immutable, they are **thread-safe**.
   - Multiple threads can share the same String instance without synchronization, improving performance.

4. **Caching and Performance**: 
   - The **hashcode** of a String is frequently used in Java (e.g., in HashMaps).
   - Since Strings are immutable, their hashcode **does not change**, allowing efficient caching and improving performance.

5. **Class Loaders**: 
   - Strings are used by Java **ClassLoaders** to load classes dynamically.
   - Immutability ensures that the correct class is loaded, preventing security risks from modified class names.

#### Conclusion
The immutability of Strings in Java improves **performance, security, thread-safety, and memory optimization**, making them a crucial part of the Java language.

---

#### What does the `equals()` method of the `String` class do?

#### Answer:
- In Java, the `Object` class is the parent of all classes, and it has an `equals()` method that **compares object references**.
- However, the `String` class **overrides** the `equals()` method to compare the **contents** of two strings instead of their references.

#### Example:

```java
public class StringEqualsExample {
    public static void main(String[] args) {
        String s1 = new String("Java");
        String s2 = new String("Java");

        System.out.println(s1 == s2); // false (compares references)
        System.out.println(s1.equals(s2)); // true (compares content)
    }
}
```

#### Explain the output of the below program related to the equals() method of StringBuilder.

```java
public class Demo {
    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder("hello");
        StringBuilder sb2 = new StringBuilder("hello");

        if (sb1.equals(sb2)) {
            System.out.println("Equal");
        } else {
            System.out.println("Not Equal");
        }
    }
}
```

#### Answer:
This is a common interview question. If you expected the output to be `Equal`, you were mistaken. The output of the above program is `Not Equal` because `StringBuilder` (and `StringBuffer`) does not override the `equals()` and `hashCode()` methods from `Object` class.

By default, `Object`'s `equals()` method is used, which checks for reference equality rather than content equality. Since `sb1` and `sb2` are different objects, the condition evaluates to false, resulting in `Not Equal` being printed.

#### Why doesn't StringBuilder override equals and hashCode?
Hash codes are used in data structures like `HashMap`, `HashSet`, `Hashtable`, and `ConcurrentHashMap`, which rely on hash-based storage. These structures require that keys do not change once inserted, so that values can be retrieved correctly using their hash codes.

Since `StringBuilder` and `StringBuffer` are **mutable**, their hash codes would change if their content changed. This makes them a poor choice for hash-based data structures, and hence, their `equals()` and `hashCode()` methods are not overridden.

#### Explain Equals and HashCode Contract:
The contract states:
- If two objects are equal according to the `equals()` method, then their `hashCode()` must also be the same.
- The reverse is **not** necessarily true: if two objects have the same hash code, they may or may not be equal.

If `StringBuilder` had overridden `equals()`, it would also need to override `hashCode()` to maintain this contract. However, as explained earlier, there is no need for `StringBuilder` to have its own `hashCode()` implementation.

#### When to use String, StringBuffer, and StringBuilder?
- **String**: Use when immutability is required.
- **StringBuffer**: Use when mutability and thread safety are required.
- **StringBuilder**: Use when mutability is required but thread safety is not needed.

#### Explain the equals and hashCode contract
The **equals and hashCode contract** states:
1. If two objects are equal according to the `equals()` method, then their `hashCode()` must also be the same.
2. The reverse is not necessarily true: if two objects have the same hash code, they may or may not be equal.

This contract ensures that objects function correctly in hash-based collections like `HashMap` and `HashSet`.

#### What is an Immutable Class. Explain with Example?
An **immutable class** is a class whose objects cannot be modified after they are created. Examples of immutable classes in Java include **String**, **Integer**, and **LocalDate**.

#### Steps to Create an Immutable Class
To make a class immutable, follow these rules:

1. **Declare the class as `final`**  
   - Prevents other classes from extending it and modifying its behavior.

2. **Make all fields `private` and `final`**  
   - Private: Prevents direct access.  
   - Final: Ensures the fields are assigned only once.

3. **Do not provide setter methods**  
   - Ensures that fields cannot be changed after object creation.

4. **Initialize fields via a constructor**  
   - Assign values to all fields in the constructor.

5. **Return deep copies of mutable objects** (if any)  
   - If the class has mutable fields (e.g., a list or date), return a copy instead of the original reference.

#### Example of an Immutable Class
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

// Declaring the class as final to prevent subclassing
public final class ImmutablePerson {
    
    // Private and final fields to ensure immutability
    private final String name;
    private final int age;
    private final List<String> hobbies; // List is mutable, so we need to handle it carefully

    // Constructor to initialize all fields
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name; // Assigning name directly (String is immutable)
        this.age = age;   // Assigning age directly (int is primitive and immutable)
        
        // Creating a new list to avoid reference leakage
        this.hobbies = new ArrayList<>(hobbies);
    }

    // Getter for name (returns the original immutable String)
    public String getName() {
        return name;
    }

    // Getter for age (returns the original immutable int)
    public int getAge() {
        return age;
    }

    // Getter for hobbies (returns an unmodifiable list to prevent modification)
    public List<String> getHobbies() {
        return Collections.unmodifiableList(hobbies);
    }
    
    // No setters are provided, ensuring immutability
}
```

#### Breakdown of Key Points in Code
| **Concept** | **How It’s Applied** |
|------------|----------------------|
| **Final Class** | `public final class ImmutablePerson` (Prevents subclass modification) |
| **Private & Final Fields** | `private final String name;` (Ensures values cannot be changed) |
| **No Setters** | No `setName()`, `setAge()`, etc. |
| **Immutable Fields** | `String` and `int` are naturally immutable |
| **Handling Mutable Fields** | Instead of storing `hobbies` directly, we create a new list: `this.hobbies = new ArrayList<>(hobbies);` |
| **Returning Safe Copies** | `Collections.unmodifiableList(hobbies)` ensures that the original list cannot be modified outside |

#### Example Usage
```java
import java.util.Arrays;
import java.util.List;

public class TestImmutable {
    public static void main(String[] args) {
        List<String> hobbies = Arrays.asList("Reading", "Gaming");
        ImmutablePerson person = new ImmutablePerson("John", 25, hobbies);
        
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
        System.out.println("Hobbies: " + person.getHobbies());
    }
}
```

#### Advantages of Immutable Classes
- **Thread-safe** (No synchronization needed)
- **Caching & Performance Optimization** (Can be safely shared)
- **Less Bug-prone** (No accidental modifications)
---


