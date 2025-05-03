#### Explain Garbage Collection in Java and its benifits?

Garbage Collection (GC) in Java is an automatic memory management process where the Java Virtual Machine (JVM) reclaims memory occupied by objects that are no longer in use. This helps in preventing memory leaks and optimizing application performance.

- the Main objective of Garbage Collector is to **identify unused objects** in order to reclaim memory space.
- It is an Automated Process of **deleting code** that is **no longer used** or needed.

#### Benefits of Garbage Collection in Java

1. **Automatic Memory Management**  
   - Java handles memory deallocation automatically using GC, reducing manual intervention.

2. **Prevention of Memory Leaks**  
   - Unused objects are identified and removed, ensuring optimal memory usage.

3. **Improved Performance**  
   - JVM optimizes GC execution to maintain application responsiveness.

4. **No Manual Deallocation**  
   - Unlike C/C++, Java does not require explicit `free()` or `delete()` calls.

5. **Enhances Security & Stability**  
   - Prevents issues like dangling pointers and memory corruption.

#### How Garbage Collection Works in Java
1. **GC identifies unreachable objects** – Objects with no references become eligible for GC.
2. **GC removes those objects** – JVM reclaims memory by cleaning up unused objects.
3. **Memory is reused** – The freed-up memory is allocated for new objects.

#### Best Practices to Optimize Garbage Collection
- Use **WeakReference** for cache objects.
- Set unused objects to `null` explicitly if they hold large memory.
- Monitor GC performance using **JVisualVM** or **Garbage Collection Logs**.
- Choose an appropriate **GC algorithm** based on your application’s needs.

#### Conclusion
Garbage Collection in Java simplifies memory management by automatically reclaiming unused memory. Understanding different GC algorithms and JVM memory areas helps in optimizing performance for large-scale applications.

---

#### What does `System.gc();` do? 
The following example demonstrates how Java garbage collection works when objects are no longer referenced:

```java
class DemoGC {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Garbage collected object: " + this);
    }
}

public class GarbageCollectionExample {
    public static void main(String[] args) {
        DemoGC obj1 = new DemoGC();
        DemoGC obj2 = new DemoGC();
        
        // Removing references
        obj1 = null;
        obj2 = null;
        
        //Suggests the Java Virtual Machine (JVM) to run the Garbage Collector (GC), but it does not guarantee that GC will execute immediately.
        System.gc();
        
        // Giving time for GC to complete
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

#### Explanation:
1. We create two objects (`obj1` and `obj2`) of the `DemoGC` class.
2. Both objects are set to `null`, making them eligible for garbage collection.
3. `System.gc();` requests garbage collection, though execution is not guaranteed.
4. The `finalize()` method is overridden to print a message when an object is garbage collected.
5. `Thread.sleep(1000);` ensures enough time for GC to run before the program exits.

#### Conclusion
Garbage Collection in Java simplifies memory management by automatically reclaiming unused memory. Understanding different GC algorithms and JVM memory areas helps in optimizing performance for large-scale applications.

---

#### What Happens When System.gc(); is Called?
- Request Sent to JVM: The call requests the JVM to perform garbage collection.
- JVM Decides Execution: The JVM may choose to ignore this request or execute it when it finds it necessary.
- Eligible Objects Are Collected: If the JVM runs the GC, it will clean up objects that are no longer referenced (like obj1 and obj2 in the example).
- Finalize Method May Run: If an object is garbage collected, its finalize() method (if overridden) will execute before the object is destroyed.

```java 
Example Output:
Garbage collected object: DemoGC@1d44bcfa
Garbage collected object: DemoGC@266474c2

```
---

#### Explain `finalize()` in Java(Depricated from Java 9) ?

The `finalize()` method in Java is a special method that is called by the garbage collector before an object is removed from memory. It was originally designed to allow objects to clean up resources before being destroyed but has been deprecated in recent Java versions due to its unreliability.

#### Syntax
```java
@Override
protected void finalize() throws Throwable {
    System.out.println("Finalize method called");
}
```

#### How It Works
- The garbage collector calls `finalize()` on an object before deallocating memory.
- Each object can override `finalize()` to perform cleanup operations.
- The method is **not guaranteed** to execute at a specific time or even at all.
- It can be called **at most once per object** by the garbage collector.

#### Example
```java
class Demo {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Finalize method executed");
    }

    public static void main(String[] args) {
        Demo obj = new Demo();
        obj = null; // Making object eligible for garbage collection
        System.gc(); // Requesting garbage collection
        System.out.println("End of main method");
    }
}
```

#### Expected Output (Not Guaranteed)
```
End of main method
Finalize method executed
```
(Note: The execution of `finalize()` depends on the JVM's garbage collection behavior.)

#### Issues with `finalize()`
- ❌ **Unreliable**: JVM decides when to run garbage collection, so `finalize()` may never execute.
- ❌ **Performance Overhead**: Slows down garbage collection if misused.
- ❌ **Deprecated in Java 9**: Java 9 officially deprecated `finalize()` due to its unpredictability.

---

#### What are the Better Alternatives after Java 9?
Instead of using `finalize()`, use the following methods:

| Approach                | Description |
|-------------------------|-------------|
| **Try-with-resources**  | Best for managing resources like files, sockets, and streams. |
| **Explicit Cleanup** (`close()`, `dispose()`) | Provides manual control over resource release. |
| **WeakReferences & java.lang.ref.Cleaner** | A safer and more efficient way to handle object cleanup. |

#### Conclusion
- `finalize()` was used for cleanup before garbage collection.
- It is **not reliable** and has been **deprecated since Java 9**.
- Always prefer **try-with-resources or explicit cleanup methods** for resource management.

---
💡 **Recommendation:** Avoid using `finalize()` and adopt modern cleanup techniques for better performance and reliability.

---

#### Explain Stack and Heap in Java ?

#### Stack Memory
- **Used for:** Storing **method-specific** data (local variables, method calls).
- **Memory Allocation:** **LIFO (Last In, First Out)** principle.
-  **Scope:** Thread-specific (Each thread has its own Stack).
- **Stores:**
   - Local primitive variables (`int`, `double`, etc.).
   - References to objects (actual objects are in Heap).
   - Method execution details (return addresses, function calls).
- **Size:** Small and grows/shrinks with method calls.
- **Access Speed:** Fast (direct memory access).
- **Garbage Collection:** No (automatically removed when method exits).

#### Example:
```java
void methodA() {
    int x = 10;  // Stored in Stack
    methodB();
}
void methodB() {
    int y = 20;  // Stored in Stack
}
```
---

#### Heap Memory
- **Used for:** Storing **objects and instance variables**.
- **Memory Allocation:** **Dynamic (grows as needed)**.
- **Scope:** Shared across all threads.
- **Stores:**
   - Objects (created via `new` keyword).
   - Instance variables (fields of objects).
- **Size:** Larger than Stack.
- **Access Speed:** Slower than Stack (requires reference lookups).
- **Garbage Collection:** **Yes**, managed by Java's **Garbage Collector**.

#### Example:
```java
class Person {
    String name;  // Stored in Heap (part of object)
    int age;      // Stored in Heap (part of object)
}
void createPerson() {
    Person p = new Person();  // 'p' is in Stack, object is in Heap
    p.name = "John";  // Stored in Heap
}
```
---

#### What are the Differences Between Stack and Heap ?

| Feature | Stack Memory | Heap Memory |
|---------|-------------|------------|
| **Used for** | Method calls, local variables | Objects, instance variables |
| **Access Speed** | Fast | Slower |
| **Scope** | Thread-specific | Shared across threads |
| **Size** | Smaller | Larger |
| **Garbage Collection** | No (auto removed on method exit) | Yes (managed by GC) |
| **Lifetime** | Short-lived | Long-lived (until GC clears it) |

Understanding the difference between **Stack and Heap Memory** is essential for optimizing memory management in Java!

---
#### Explain How Stack work in Java for Method calls ?

#### Method Calls and Stack Frames

#### Code Example
```java
void methodA() {
    int x = 10;  // Stored in Stack
    methodB();   // Calls methodB()
}

void methodB() {
    int y = 20;  // Stored in Stack
}
```

#### Explanation
- Each method call creates a **new stack frame**.
- Local variables (`x` and `y`) are stored in their respective frames.
- When a method **finishes execution**, its stack frame is removed.
- The stack **follows LIFO (Last In, First Out)** principle.

#### Summary
| Feature           | Stack Memory |
|------------------|--------------|
| **Used for**    | Method calls, local variables |
| **Access Speed** | Fast |
| **Scope**       | Thread-specific |
| **Size**        | Smaller |
| **Garbage Collection** | No (auto removed on method exit) |
| **Lifetime**    | Short-lived |

---

#### Explain Java Heap Memory ?
Java Heap is the memory area where objects are dynamically allocated. It is shared among all threads and managed by the **Garbage Collector (GC)**.

#### Key Features:
- Stores **objects** and **class instances**.
- Managed automatically by **Garbage Collection**.
- **Shared** across multiple threads.
- Objects persist **until they become unreachable**.

---

#### Visualizing Heap Memory Allocation
Consider the following Java code:

```java
class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class HeapExample {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 25);
        Person p2 = new Person("Bob", 30);
    }
}
```

---
#### Explain Strong Reference vs Weak Reference in Java?

In Java, references determine how objects are stored and garbage collected. The two most common reference types are **Strong Reference** and **Weak Reference**.

---

#### 1. Strong Reference (Default Reference in Java)
A **strong reference** is the normal way objects are referenced in Java. As long as a strong reference exists, the object **will not be garbage collected**.

#### Example of Strong Reference
```java
class StrongRefExample {
    public static void main(String[] args) {
        // Creating a strong reference
        String strongRef = new String("Hello, Java!");

        // Making it null, now it's eligible for GC
        strongRef = null;

        // Suggesting Garbage Collection
        System.gc();

        System.out.println("End of program");
    }
}
```
#### Explanation
- Objects with strong references are **not garbage collected** until explicitly set to `null`.  
- This is the default way objects are referenced in Java.  

---

#### 2. Weak Reference
A **weak reference** allows objects to be garbage collected **even if they are still referenced somewhere**. This is useful in cases like caching where objects should be removed when memory is needed.

#### Example of Weak Reference
```java
import java.lang.ref.WeakReference;

class WeakRefExample {
    public static void main(String[] args) {
        // Strong Reference
        String strongRef = new String("Hello, WeakReference!");

        // Creating a Weak Reference
        WeakReference<String> weakRef = new WeakReference<>(strongRef);

        // Removing Strong Reference
        strongRef = null;

        // Suggest Garbage Collection
        System.gc();

        // Trying to access weak reference
        System.out.println("Weak Reference: " + weakRef.get()); // May print "null" if GC has collected it
    }
}
```
#### Explanation
- If there is no strong reference, GC will remove the object.  
- Used for **caching** and **memory-sensitive applications**.  

---

#### What are the Differences Between Strong and Weak References?

| Feature | **Strong Reference** | **Weak Reference** |
|---------|------------------|----------------|
| **Garbage Collection** | Object is **not** eligible for GC if strongly referenced | Object is eligible for GC even if referenced |
| **Use Case** | Normal objects that should persist in memory | Objects that can be removed when memory is low (caching, maps) |
| **Performance Impact** | Can cause **memory leaks** if not set to `null` properly | Helps in **efficient memory management** |
| **Example** | `String str = new String("Hello");` | `WeakReference<String> weakStr = new WeakReference<>(str);` |

---

#### Real-Time Example: Caching System Using `WeakHashMap`
#### Scenario
Imagine you are building a **user session management system** where user sessions are stored in a cache. However, you don’t want inactive sessions to occupy memory forever. If a session is no longer referenced, it should be **automatically removed**.

#### Example Code
```java
import java.util.Map;
import java.util.WeakHashMap;

class User {
    String name;

    User(String name) {
        this.name = name;
    }

    @Override
    protected void finalize() throws Throwable {
        System.out.println(name + " is garbage collected");
    }
}

public class WeakHashMapExample {
    public static void main(String[] args) throws InterruptedException {
        // Using WeakHashMap to store user sessions
        Map<User, String> userCache = new WeakHashMap<>();

        User user1 = new User("Alice");
        User user2 = new User("Bob");

        // Adding users to the cache
        userCache.put(user1, "Session1");
        userCache.put(user2, "Session2");

        System.out.println("Before GC: " + userCache);

        // Remove strong references
        user1 = null;
        user2 = null;

        // Suggesting garbage collection
        System.gc();

        // Waiting for GC to run
        Thread.sleep(2000);

        System.out.println("After GC: " + userCache);
    }
}
```
#### Output (May Vary)
```
Before GC: {Alice=Session1, Bob=Session2}
Alice is garbage collected
Bob is garbage collected
After GC: {}
```

#### Explanation
- **Before GC**: The `WeakHashMap` holds weak references to the `User` objects.
- **After GC**: Since `user1` and `user2` were set to `null`, they became **eligible for garbage collection**, and `WeakHashMap` automatically removed them.

---

#### When to Use Each Reference Type?

| **Use Case** | **Strong Reference** | **Weak Reference** |
|-------------|------------------|----------------|
| **Normal Object Usage** | ✅ Yes | ❌ No |
| **Caching (e.g., User Sessions, Temporary Objects)** | ❌ No | ✅ Yes |
| **Preventing Memory Leaks** | ❌ No | ✅ Yes |
| **Collections (`WeakHashMap`)** | ❌ No | ✅ Yes |

---
#### Explain Young Generation vs Old Generation in Heap Memory ?

In Java, heap memory is divided into different regions to optimize garbage collection. The two main sections are:

1. **Young Generation (YoungGen)**
2. **Old Generation (OldGen)**

#### Young Generation (YoungGen)
- Stores **newly created objects**.
- Divided into three parts:
  - **Eden Space**: Where new objects are allocated.
  - **Survivor Space S0 (Survivor 0)**
  - **Survivor Space S1 (Survivor 1)**
- Uses **Minor Garbage Collection (Minor GC)**, which is frequent and fast.
- Objects that survive multiple Minor GCs move to the **Old Generation**.

#### Old Generation (OldGen)
- Stores **long-lived objects** that survived multiple Minor GCs.
- Uses **Major Garbage Collection (Major GC) / Full GC**, which is less frequent but slower.
- If OldGen is full, an **OutOfMemoryError** occurs.

#### Memory Flow Example
| Step | Object Location | Action |
|------|---------------|--------|
| 1 | New Object | Allocated in **Eden Space** |
| 2 | Minor GC | Surviving objects move to **Survivor S0** |
| 3 | Another Minor GC | Surviving objects move to **Survivor S1** |
| 4 | Multiple Minor GCs | Objects that survive move to **Old Generation** |
| 5 | Major GC | Cleans up long-lived objects in **Old Generation** |

#### Key Differences Between YoungGen and OldGen
| Feature | Young Generation | Old Generation |
|---------|-----------------|---------------|
| Object Type | Short-lived | Long-lived |
| Garbage Collection | Minor GC (fast) | Major GC / Full GC (slow) |
| Frequency | Frequent | Less frequent |
| Subdivisions | Eden, Survivor S0, Survivor S1 | No subdivisions |
| Effect on Performance | Less impact | Can cause application pauses |

#### Code Example
```java
public class MemoryDemo {
    public static void main(String[] args) {
        // Step 1: Creating a large number of short-lived objects
        for (int i = 0; i < 100_000; i++) {
            String temp = new String("Temporary Object " + i);
        } // These objects will be collected by Minor GC

        // Step 2: Creating a long-lived object
        String[] longLivedObjects = new String[100_000];
        for (int i = 0; i < longLivedObjects.length; i++) {
            longLivedObjects[i] = new String("Persistent Object " + i);
        }
        
        // These objects will move to Old Generation after surviving multiple GCs
        System.out.println("Objects allocated successfully!");
    }
}
```
---
#### Where are Objects Created in Java?
In **Java**, objects are **always** created in the **Heap Memory**, while references (variables pointing to objects) are stored in the **Stack Memory**.

---

#### **Example: Object Storage in Heap and Stack**

```java
class Person {
    String name;
}

public class MemoryDemo {
    public static void main(String[] args) {
        Person p1 = new Person();  // "new Person()" creates an object in the Heap.
        Person p2 = p1;  // p2 also stores reference to the same Heap object.
    }
}
```

#### **Memory Allocation Breakdown**

| Memory Type | Stored Items |
|------------|-------------|
| **Heap** | `new Person()` (actual object) |
| **Stack** | `p1` (reference to the object) |
| **Stack** | `p2` (another reference to the same object) |

---

#### Who Manages the Garbage Collector?
The **Garbage Collector (GC)** in Java is managed by the **Java Virtual Machine (JVM)**.

#### Key Points
- The **JVM** is responsible for automatically handling memory management.
- The **Garbage Collector** runs as a **background thread**, reclaiming memory from objects that are no longer reachable.
- The **developer cannot force** garbage collection but can suggest it using `System.gc()` or `Runtime.getRuntime().gc()`. However, execution is **not guaranteed**.

---


#### When Do Objects Become Eligible for Garbage Collection?
An object in Java becomes **eligible for garbage collection (GC)** when it is no longer reachable by any active thread or static reference. Below are the different ways objects can become eligible for GC.

---
#### different Ways 
- Explicit `null` assignment
- Reassigning references
- Method-local objects
- Island of isolation
- Anonymous objects
- Weak references
- Class unloading

#### 1. Nullifying the Reference
If a reference is explicitly set to `null`, the object becomes unreachable.

```java
class Test {
    public static void main(String[] args) {
        String s = new String("Hello");
        s = null;  // Now "Hello" object is eligible for GC
    }
}
```

---

#### 2. Reassigning a Reference Variable
If an object reference is reassigned to another object, the old object becomes unreachable.

```java
class Test {
    public static void main(String[] args) {
        String s1 = new String("Java");
        s1 = new String("Python");  // "Java" object is now eligible for GC
    }
}
```

---

#### 3. Objects Inside Method Scope (Local Objects)
Objects created inside a method become unreachable once the method finishes execution.

```java
class Test {
    void method() {
        String s = new String("Temporary");  // Eligible for GC after method exits
    }

    public static void main(String[] args) {
        Test t = new Test();
        t.method();  
    }
}
```

---

#### 4. Island of Isolation
Two or more objects referencing each other but not referenced by any active thread form an **island of isolation** and become eligible for GC.

```java
class Test {
    Test t;
    public static void main(String[] args) {
        Test t1 = new Test();
        Test t2 = new Test();

        t1.t = t2;
        t2.t = t1;

        t1 = null;
        t2 = null;  // Now both objects are unreachable and eligible for GC
    }
}
```

---

#### 5. Anonymous Objects
Objects created without references become eligible for GC immediately after use.

```java
class Test {
    public static void main(String[] args) {
        new String("Anonymous");  // Eligible for GC immediately after execution
    }
}
```

---

#### 6. Weak References (Explicit GC)
Objects referenced only by **WeakReference** or **SoftReference** may be garbage collected when memory is needed.

```java
import java.lang.ref.WeakReference;

class Test {
    public static void main(String[] args) {
        WeakReference<String> weakRef = new WeakReference<>(new String("Weak Reference"));
        System.gc();  // "Weak Reference" object may be GC'd
    }
}
```

---

#### 7. Class Unloading (Static Reference Removal)
If a class loader is removed and all static references are cleared, its loaded classes and static objects may be garbage collected.

---

#### **Is the Garbage Collector a Foreground or Background Thread?**
The **Garbage Collector (GC) in Java runs as a background thread**. It is a **low-priority daemon thread** managed by the JVM.

---

The following code demonstrates when the garbage collector runs:

```java
class GCExample {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Garbage Collector is running...");
    }

    public static void main(String[] args) {
        GCExample obj = new GCExample();
        obj = null; // Make object eligible for GC
        System.gc(); // Suggest GC execution
    }
}
```

**Output (if GC runs):**
```bash
Garbage Collector is running...
```
---

#### What are the benifits of MetaSpace in java 8 ?

##### PermGen Memory
- **PermGen (Permanent Generation)** is a special space in the Java heap, separated from the main memory.
- It stores **static content** and **application metadata** required by the JVM.
- **Metadata** is data used to describe other data.
- **Garbage collection** occurs in PermGen like any other memory region.
- **String pool** was stored in PermGen before Java 7.
- The **Method Area** is part of PermGen, used for storing class structure and method/constructor code.
- **Disadvantages of PermGen:**
  - **Fixed size** leads to `OutOfMemoryError`.
  - **Default size**: 64 MB (32-bit JVM) and 82 MB (64-bit JVM).
  - **Frequent garbage collection** increases JVM overhead.
  - **Manual resizing** is possible, but PermGen **cannot auto-increase**.
  - **Garbage collection is inefficient** in cleaning PermGen memory.

##### MetaSpace (Introduced in Java 8)
- **MetaSpace replaces PermGen** from Java 8 onwards.
- **MetaSpace grows automatically by default**, unlike PermGen.
- **Garbage collection** is triggered automatically **when class metadata usage reaches the maximum MetaSpace size**.
- **Solves PermGen limitations**, offering better performance and flexibility.

By replacing **PermGen with MetaSpace**, Java 8 improves memory management, reduces `OutOfMemoryError` risks, and optimizes garbage collection efficiency.

# Types of Garbage Collectors in Java

Java provides several types of **Garbage Collectors**, each optimized for different use cases:

---

#### **1. Serial Garbage Collector**  
- Uses a **single thread** for garbage collection.  
- Best suited for **small applications** running on **single-core CPUs**.  
- **JVM Option:** `-XX:+UseSerialGC`

---

#### **2. Parallel Garbage Collector (Throughput Collector)**  
- Uses **multiple threads** to perform garbage collection.  
- Focuses on **maximizing application throughput** by minimizing the time spent in garbage collection.  
- Best for **multi-core processors** with **medium to large heaps**.  
- **JVM Option:** `-XX:+UseParallelGC`

---

#### **3. CMS (Concurrent Mark-Sweep) Garbage Collector** (Deprecated in Java 9, Removed in Java 14)  
- Runs **mostly concurrently** with the application to **reduce stop-the-world pauses**.  
- Reduces **latency** but has **higher CPU overhead**.  
- **JVM Option:** `-XX:+UseConcMarkSweepGC`

---

#### **4. G1 (Garbage First) Garbage Collector** (Default from Java 9 onwards)  
- Designed for **low-latency applications** with **large heaps**.  
- Divides the heap into **regions** and prioritizes garbage collection in regions with the most garbage.  
- Reduces **full GC pauses** by **incremental cleanup**.  
- **JVM Option:** `-XX:+UseG1GC`

---

#### **5. Z Garbage Collector (ZGC)** (Introduced in Java 11, Improved in later versions)  
- **Ultra-low-latency GC** with **pause times under 10ms**, even for **large heaps (TB scale)**.  
- Performs **most work concurrently** without stopping the application.  
- **JVM Option:** `-XX:+UseZGC`

---

#### **6. Shenandoah Garbage Collector** (Introduced in Java 12)  
- Similar to ZGC, designed for **low-pause-time garbage collection**.  
- Uses **concurrent compaction** to keep pauses very short.  
- **JVM Option:** `-XX:+UseShenandoahGC`

---

#### **Comparison Table**


<table>
  <thead>
    <tr>
      <th>Garbage Collector</th>
      <th>Best For</th>
      <th>Pause Time</th>
      <th>Heap Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Serial GC</strong></td>
      <td>Small apps, single-threaded programs</td>
      <td>High</td>
      <td>Small</td>
    </tr>
    <tr>
      <td><strong>Parallel GC</strong></td>
      <td>High-throughput apps, batch processing</td>
      <td>Medium</td>
      <td>Medium to Large</td>
    </tr>
    <tr>
      <td><strong>CMS GC</strong> <em>(Deprecated)</em></td>
      <td>Low-latency apps (before Java 9)</td>
      <td>Low</td>
      <td>Medium to Large</td>
    </tr>
    <tr>
      <td><strong>G1 GC</strong> <em>(Default since Java 9)</em></td>
      <td>General-purpose, balanced performance</td>
      <td>Low</td>
      <td>Large</td>
    </tr>
    <tr>
      <td><strong>ZGC</strong></td>
      <td>Large heap, ultra-low-latency apps</td>
      <td>Very Low (&lt;10ms)</td>
      <td>Huge (TB-scale)</td>
    </tr>
    <tr>
      <td><strong>Shenandoah GC</strong></td>
      <td>Low-latency workloads</td>
      <td>Very Low</td>
      <td>Medium to Large</td>
    </tr>
  </tbody>
</table>

---

#### What are the Differences between Serial Garbage Collector and Parallel Garbage Collector ?

| Feature | Serial Garbage Collector | Parallel Garbage Collector |
|---------|-------------------------|----------------------------|
| **Thread Usage** | Uses a **single thread** | Uses **multiple threads** |
| **Best For** | Small applications, single-core CPUs | Large applications, multi-core CPUs |
| **Pause Time** | Higher pause time | Lower pause time due to parallelism |
| **Throughput** | Lower, as only one thread works at a time | Higher, as multiple threads share the workload |
| **JVM Option** | `-XX:+UseSerialGC` | `-XX:+UseParallelGC` |

---

#### **Why G1 Replaced Parallel GC as the Default (Java 9+)**
| Feature | Parallel GC (Before Java 9) | G1 GC (Default from Java 9) |
|---------|----------------------------|----------------------------|
| **Pause Time** | Unpredictable, can be long | More predictable, lower pauses |
| **Heap Management** | Fixed generations (Young, Old) | Dynamically divided into **regions** |
| **Stop-the-World Events** | Long for large heaps | Splits GC work into smaller tasks |
| **Compaction** | Performed during full GC | **Concurrent compaction**, reducing pauses |

---

#### **How G1 GC Divides the Heap into Regions Dynamically?**

Unlike **Parallel GC**, which splits the heap into **fixed generations**, **G1 GC divides the heap into equal-sized regions** dynamically and assigns them roles based on application behavior.

#### **Heap Structure in G1 GC**
| **Region Type** | **Purpose** |
|---------------|------------|
| **Eden** | Stores newly allocated objects (short-lived) |
| **Survivor** | Holds objects surviving minor GCs before promotion to Old Gen |
| **Old** | Stores long-lived objects |
| **Humongous** | Stores very large objects that don't fit in a single region |
| **Unused (Free)** | Empty regions available for allocation |

#### **How G1 GC Works Dynamically?**
1. **Initial Allocation**: Eden, Survivor, and Old regions are assigned at startup.
2. **Dynamic Region Adjustment**: G1 **monitors object survival rates** and **adjusts region sizes dynamically**.
3. **Garbage Collection Process**:
   - **Young GC (Minor GC)**: Cleans **Eden and Survivor** regions.
   - **Mixed GC**: Collects **both Young and some Old** regions selectively.
   - **Concurrent GC**: Runs in the background to clean Old Gen without stopping the application.
4. **Humongous Object Handling**: Objects larger than 50% of a region are stored in **Humongous Regions**.
5. **Heap Compaction (Automatic Defragmentation)**: Moves live objects to free up fragmented space, reducing the need for Full GC.

---

#### Synchronous vs Asynchronous Programming in Concurrency & Parallelism

| Feature         | **Synchronous** | **Asynchronous** |
|---------------|----------------|------------------|
| **Execution** | Tasks run one after another, blocking the thread until completion. | Tasks can run independently, without blocking the thread. |
| **Concurrency** | Limited concurrency; each task must finish before the next one starts. | Supports high concurrency by scheduling tasks to run when resources are available. |
| **Parallelism** | Typically single-threaded unless used with multi-threading. | Can achieve parallelism when combined with multi-threading. |
| **Thread Usage** | Usually single-threaded, but can use multiple threads explicitly. | Can use multiple threads, event loops, or callbacks to improve efficiency. |
| **Example** | Reading files one by one in sequence. | Reading multiple files simultaneously using non-blocking I/O. |

---

