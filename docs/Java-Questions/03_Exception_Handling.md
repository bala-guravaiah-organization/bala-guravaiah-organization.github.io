#### What is an Exception and Exception Handling?

An **exception** is an event that **disrupts** the normal flow of a program. It is an **object** that is thrown at runtime.  
**Exception Handling** is the process of **managing** these exceptions to ensure the program continues to run smoothly.

---

#### Example: Exception being thrown

```java
public class ExceptionExample {
    public static void main(String[] args) {
        int result = 10 / 0;  // Throws ArithmeticException
        System.out.println("Result: " + result);
    }
}
```

**Output:**
```
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at ExceptionExample.main(ExceptionExample.java:3)
```

---

#### Example: Exception Handling using `try-catch`

```java
public class ExceptionHandlingExample {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // Throws ArithmeticException
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
        System.out.println("Program continues...");
    }
}
```

**Output:**
```
Exception caught: / by zero
Program continues...
```

---

#### Difference between **Error** and **Exception**

#### **Error:**
- **Irrecoverable** and indicates serious issues.
- Causes program **termination**.
- **Examples:**  
  - `OutOfMemoryError` (Running out of memory)
  - `StackOverflowError` (Excessive recursion)

#### **Exception:**
- **Recoverable** using **exception handling**.
- **Examples:**  
  - `NullPointerException` (Accessing methods on `null`)
  - `ArithmeticException` (Division by zero)

---

#### Types of Exceptions

#### **1. Checked Exceptions**  
- Enforced by the **compiler** at **compile-time**.  
- **Examples:** `IOException`, `SQLException`, `FileNotFoundException`

#### **2. Unchecked Exceptions**  
- Occur at **runtime** and are **not checked** by the compiler.  
- **Examples:** `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`

---

#### How is exception handling done in Java?

Exception handling in Java is done using the `try-catch` block. If you think that certain statements may throw an exception, surround them with a `try` block.

#### ✅ Key Points to Remember:
- A `try` block is always followed by a `catch` block, a `finally` block, or both.
- You cannot use a `try` block alone.

#### Example:
```java
try {
    // Code that may throw an exception
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // Handling the exception
    System.out.println("Cannot divide by zero");
} finally {
    // This block always executes
    System.out.println("Execution completed");
}
```

#### Can we write a try block without a catch block?

Yes, we can write a `try` block with a `finally` block, but we cannot write a `try` block alone.

#### ✅ Key Points to Remember:
- A `try` block must be followed by either a `catch` block, a `finally` block, or both.
- If a `finally` block is present, it will always execute, even if an exception occurs.

#### Question 6: How to handle multiple exceptions together?

You can write multiple `catch` blocks one after another for each exception, or you can write a single `catch` block using a pipe symbol (`|`) to separate the exceptions.

#### ✅ Key Rules to Remember:
- Handle the **most specific exception first**, then move down to the most generic ones.
  - Example: You **cannot** handle `Exception` (the base class) before `FileNotFoundException`.
- If your method throws multiple exceptions and you want to perform specific actions based on the exception thrown, use **multiple catch blocks**.
- Use the **pipe (`|`) symbol** to handle multiple exceptions in a single catch block if they require the same handling.

#### Example using multiple catch blocks:
```java
try {
    // Code that may throw multiple exceptions
    int arr[] = new int[5];
    arr[10] = 50 / 0; // May throw ArithmeticException or ArrayIndexOutOfBoundsException
} catch (ArithmeticException e) {
    System.out.println("ArithmeticException: Division by zero");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("ArrayIndexOutOfBoundsException: Index out of bounds");
} catch (Exception e) {
    System.out.println("General Exception: " + e.getMessage());
}
```

#### Example using pipe (`|`) symbol:
```java
try {
    // Code that may throw multiple exceptions
    String s = null;
    System.out.println(s.length());
} catch (NullPointerException | ArithmeticException e) {
    System.out.println("Exception occurred: " + e.getMessage());
}
```

#### When will the finally block not get executed?

The `finally` block will not execute in the following cases:
- When `System.exit()` is called.
- When the JVM crashes.

#### Difference between `throw` and `throws` keyword & Exception Propagation

#### ✅ Key Differences Between `throw` and `throws`:
- `throw` is used to **explicitly throw an exception** inside a function or a block of code.
- `throws` is used with the **method signature** to declare exceptions that might be thrown while executing the code.
- `throw` is followed by an **instance** of an exception class, whereas `throws` is followed by **exception class names**.
- You can throw **one exception at a time** using `throw`, but you can **declare multiple exceptions** using `throws`.
- Using `throw`, only **unchecked exceptions** are propagated, whereas using `throws`, **both checked and unchecked exceptions** can be propagated.

#### Example:
#### Using `throw`:
```java
public void checkAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be 18 or above");
    }
    System.out.println("Eligible to vote");
}
```

#### Using `throws`:
```java
public void readFile() throws IOException {
    FileReader file = new FileReader("test.txt");
    BufferedReader fileInput = new BufferedReader(file);
    throw new IOException("File not found");
}
```

#### Exception Propagation

#### ✅ Key Points:
- An exception is first thrown from the top of the stack.
- If it is not caught, it drops down the call stack to the previous method.
- This continues until the exception is caught or reaches the bottom of the call stack.
- This is called **Exception Propagation**.

#### Example of Exception Propagation:
```java
class Test {
    void method3() {
        int result = 10 / 0; // Throws ArithmeticException
    }
    void method2() {
        method3();
    }
    void method1() {
        try {
            method2();
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        }
    }
    public static void main(String args[]) {
        Test obj = new Test();
        obj.method1();
    }
}
```

#### Checked Exceptions and Propagation:
Checked exceptions are **not propagated** down the call chain by default. If you want to propagate them, you **must use the `throws` keyword**.

#### Example of Checked Exception Propagation:
```java
class TestCheckedException {
    void method3() throws IOException {
        throw new IOException("Device error");
    }
    void method2() throws IOException {
        method3();
    }
    void method1() {
        try {
            method2();
        } catch (IOException e) {
            System.out.println("Exception caught: " + e);
        }
    }
    public static void main(String args[]) {
        TestCheckedException obj = new TestCheckedException();
        obj.method1();
    }
}
```

#### Unchecked Exceptions and Propagation:
Unchecked exceptions **are propagated by default**.

#### Example of Unchecked Exception Propagation:
```java
class TestUncheckedException {
    void method3() {
        int data = 10 / 0; // Throws ArithmeticException
    }
    void method2() {
        method3();
    }
    void method1() {
        try {
            method2();
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        }
    }
    public static void main(String args[]) {
        TestUncheckedException obj = new TestUncheckedException();
        obj.method1();
    }
}
```

#### Exception Handling with Method Overriding

#### **Rules:**
1. **If the parent class method does not declare an exception, the child class cannot throw a checked exception.**
   
   ```java
   class Parent {
       public void hello() {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() throws IOException { // ❌ Compilation Error
           System.out.println("Child class hello method");
       }
   }
   ```

   **Error:**
   ```
   Exception IOException is not compatible with throws clause in Parent.hello()
   ```

2. **A child class can throw an unchecked exception even if the parent class method does not declare one.**

   ```java
   class Parent {
       public void hello() {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() throws ArithmeticException { // ✅ Allowed
           System.out.println("Child class hello method");
       }
   }

   public class TestException {
       public static void main(String[] args) {
           Parent p = new Child();
           p.hello();
       }
   }
   ```

   **Output:**
   ```
   Child class hello method
   ```

3. **If a child class method throws a broader exception than the parent class method, it results in a compilation error.**

   ```java
   class Parent {
       public void hello() throws ArithmeticException {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() throws Exception { // ❌ Compilation Error
           System.out.println("Child class hello method");
       }
   }
   ```

   **Error:**
   ```
   Exception Exception is not compatible with throws clause in Parent.hello()
   ```

4. **A child class can throw the same exception as the parent class.**

   ```java
   class Parent {
       public void hello() throws Exception {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() throws Exception { // ✅ Allowed
           System.out.println("Child class hello method");
       }
   }

   public class TestException {
       public static void main(String[] args) {
           Parent p = new Child();
           try {
               p.hello();
           } catch (Exception e) {
               System.out.println("Handled");
           }
       }
   }
   ```

   **Output:**
   ```
   Child class hello method
   ```

5. **A child class method can declare a narrower exception than the parent class method.**

   ```java
   import java.io.IOException;

   class Parent {
       public void hello() throws Exception {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() throws IOException { // ✅ Allowed
           System.out.println("Child class hello method");
       }
   }

   public class TestException {
       public static void main(String[] args) {
           Parent p = new Child();
           try {
               p.hello();
           } catch (Exception e) {
               System.out.println("Handled");
           }
       }
   }
   ```

   **Output:**
   ```
   Child class hello method
   ```

6. **If the parent class method declares an exception, the child class can override it without declaring an exception.**

   ```java
   class Parent {
       public void hello() throws Exception {
           System.out.println("Parent class hello method");
       }
   }

   class Child extends Parent {
       public void hello() { // ✅ Allowed (No exception)
           System.out.println("Child class hello method");
       }
   }

   public class TestException {
       public static void main(String[] args) {
           Parent p = new Child();
           try {
               p.hello();
           } catch (Exception e) {
               System.out.println("Handled");
           }
       }
   }
   ```

   **Output:**
   ```
   Child class hello method
   ```

---

#### Programs related to Exception Handling and `return` keyword

#### ✅ Key Points to Remember:
- If you write anything after the `return` statement or `throw` exception statement, it will cause a **compile-time error** due to **unreachable code**.
- In a `try-catch-finally` block, if the `finally` block contains a `return` statement, it will **override** any return values from the `try` or `catch` block.

#### Example Code:
```java
package com.tech;

public class DemoException {
    public static int method1() {
        try {
            throw new ArithmeticException();
            return 1; // <-- Error: Unreachable code
        } catch(Exception e) {
            return 2;
        } finally {
            return 3;
        }
    }

    public static void main(String[] args) {
        int result = method1();
        System.out.println(result);
    }
}
```

#### ✅ Expected Output:
```
3
```

#### Explanation:
- The `try` block throws an `ArithmeticException`, skipping the `return 1;` statement.
- The `catch` block catches the exception and returns `2`.
- However, the `finally` block **always executes** and returns `3`, overriding the `catch` block's return value.
- Thus, the output is `3`.

---

#### `finally` block is always executed
```java
package com.tech;

public class DemoException {
    public static int method1() {
        try {
            return 1; // This value is overridden by the finally block
        } catch(Exception e) {
            return 2;
        } finally {
            return 3; // This return always executes
        }
    }

    public static void main(String[] args) {
        int result = method1();
        System.out.println(result); // Output: 3
    }
}
```

---

#### Program execution returns from the `finally` block
```java
public class DemoException {
    public static int method1() {
        try {
            return 1;
        } catch(Exception e) {
            return 2;
        } finally {
            return 3;
        }
        return 4; // <-- Error: Unreachable code (never executes)
    }

    public static void main(String[] args) {
        int result = method1();
        System.out.println(result); // Output: 3
    }
}
```

---

#### Exception thrown inside `try`, but overridden by `finally`
```java
public class DemoException {
    public static int method1() {
        try {
            int a = 15/0; // Throws ArithmeticException
            return 1;      // Unreachable due to exception
        } catch(Exception e) {
            return 2;      // Overridden by finally block
        } finally {
            return 3;       // Always executes, final return value
        }
    }

    public static void main(String[] args) {
        int result = method1();
        System.out.println(result); // Output: 3
    }
}
```

---

#### Exception thrown inside `try`, handled in `catch`
```java
public class DemoException {
    public static int method1() {
        try {
            int a = 15/0; // Throws ArithmeticException
            return 1;      // Unreachable due to exception
        } catch(Exception e) {
            return 2;      // Executed and returns 2
        }
    }

    public static void main(String[] args) {
        int result = method1();
        System.out.println(result); // Actual Output: 2
    }
}
```

#### How to make your own custom exception class?

#### ✅ Key Points to Remember:
- In Java, you can create a **custom exception class** by extending the `Exception` class.
- If you extend `Exception`, it becomes a **checked exception**.
- If you extend `RuntimeException`, it becomes an **unchecked exception**.
- Always provide a meaningful message in the constructor.

#### Example Code:
```java
// Creating a custom exception by extending Exception class
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

// Using the custom exception
public class CustomExceptionDemo {
    public static void validateAge(int age) throws CustomException {
        if (age < 18) {
            throw new CustomException("Age must be 18 or above.");
        } else {
            System.out.println("Valid age.");
        }
    }

    public static void main(String[] args) {
        try {
            validateAge(16);
        } catch (CustomException e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }
}
```

#### ✅ Expected Output:
```
Exception caught: Age must be 18 or above.
```

---

#### What happens when you throw an exception from the `finally` block?

#### ✅ Key Points to Remember:
- When an exception is thrown from the `finally` block, it **takes precedence** over exceptions thrown from the `try` or `catch` block.
- This means any previously thrown exceptions from `try` or `catch` **get suppressed**, and the exception from `finally` is propagated.

#### Example Code:
```java
public class FinallyExceptionDemo {
    public static void method() throws Exception {
        try {
            throw new Exception("Exception from try block");
        } catch (Exception e) {
            throw new Exception("Exception from catch block");
        } finally {
            throw new Exception("Exception from finally block");
        }
    }

    public static void main(String[] args) {
        try {
            method();
        } catch (Exception e) {
            System.out.println("Caught: " + e.getMessage());
        }
    }
}
```

#### ✅ Expected Output:
```
Caught: Exception from finally block
```

---

#### What is the Output of the following `try-catch-finally` program

#### Example Code:
```java
class MyException1 extends Exception { }
class MyException2 extends Exception { }

public class DemoException {
    public static void method1() throws Exception {
        try {
            System.out.println("5");
            throw new MyException1(); // Throws MyException1
        } catch (Exception e) {
            System.out.println("6");
            throw new MyException2(); // Throws MyException2 (overridden by finally block)
        } finally {
            System.out.println("7");
            throw new Exception(); // Finally block throws generic Exception (overrides previous exceptions)
        }
    }

    public static void main(String[] args) throws Exception {
        try {
            System.out.println("1");
            method1(); // Exception propagates from here
            System.out.println("2"); // Unreachable
        } catch (Exception e) {
            System.out.println("3");
            throw new MyException2(); // Overridden by finally block
        } finally {
            System.out.println("4");
            throw new MyException1(); // Final exception thrown to JVM
        }
    }
}
```

#### ✅ Expected Output:
```
1
5
6
7
4
Exception in thread "main" MyException1
```

#### Explanation:
- Execution starts in `main()` and prints `1`.
- `method1()` is called, which prints `5` and throws `MyException1`.
- The `catch` block catches `MyException1`, prints `6`, and throws `MyException2`.
- The `finally` block executes, prints `7`, and throws a generic `Exception`, overriding the previous exceptions.
- In `main()`, the `finally` block prints `4` and throws `MyException1`, which is the final exception thrown to the JVM.

---

#### Explain `try-with-resources`

#### ✅ Key Points to Remember:
- Introduced in **Java 7**, `try-with-resources` ensures that resources (like files, sockets, or database connections) are **automatically closed**.
- The resources **must implement the `AutoCloseable` interface**.
- This helps **avoid memory leaks** and **simplifies exception handling**.

#### Syntax:
```java
try (ResourceType resource = new ResourceType()) {
    // Use resource
} catch (Exception e) {
    // Handle exceptions
}
```

#### Example Code:
```java
import java.io.*;

public class TryWithResourcesDemo {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}
```

#### ✅ Expected Output (if `test.txt` contains "Hello World"):
```
Hello World
```

#### Explanation:
- `BufferedReader` is declared inside the `try` block.
- Once the `try` block finishes, the `BufferedReader` **automatically closes**, even if an exception occurs.
- No need to explicitly call `br.close()`. Java handles it internally.

---

#### Exception Hierarchy in Java

#### ✅ Key Points to Remember:
- All exceptions in Java **extend from `Throwable`**.
- `Throwable` has two direct subclasses:
  - `Exception` (Checked exceptions)
  - `Error` (Serious system failures)
- `RuntimeException` is a subclass of `Exception` (Unchecked exceptions).

#### Exception Hierarchy Diagram:
```
Throwable
│
├── Exception
│   ├── IOException
│   ├── SQLException
│   ├── RuntimeException
│       ├── NullPointerException
│       ├── ArithmeticException
│       ├── ArrayIndexOutOfBoundsException
│
└── Error
    ├── StackOverflowError
    ├── OutOfMemoryError
```

#### Explanation:
- **Checked exceptions** (`IOException`, `SQLException`) **must be handled**.
- **Unchecked exceptions** (`NullPointerException`, `ArithmeticException`) **do not need explicit handling**.
- **Errors** (`StackOverflowError`, `OutOfMemoryError`) indicate system-level failures.

---
