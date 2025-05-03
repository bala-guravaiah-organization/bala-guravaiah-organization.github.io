#### Multithreading vs Multiprocessing

#### What is Multiprocessing?
Multiprocessing is when multiple **processes** run at the same time. Each process has its own memory space, making it more powerful but also more resource-intensive compared to multithreading.

#### Example: Restaurant Analogy 🍽️
- Imagine a restaurant with **multiple kitchens**, each with its own chef.
- Each chef works independently, making the process faster but requiring more space and staff.

#### Example in Java:
```java
import java.io.IOException;

public class MultiprocessingExample {
    public static void main(String[] args) {
        try {
            ProcessBuilder pb = new ProcessBuilder("notepad.exe"); // Opens a new process
            pb.start();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
#### What is Multithreading?
Multithreading is a technique where multiple **threads** run within a single **process**. These threads share the same memory space but execute independently, allowing programs to perform multiple operations simultaneously.

#### Example: Restaurant Analogy 🍽️
- Imagine a restaurant with **one kitchen** and **multiple waiters**.
- Each waiter handles a different order (task), but they all use the same kitchen (shared memory).
- If the kitchen is busy, a waiter might have to wait.

#### Example in Java:
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getId() + " is running");
    }
}

public class MultithreadingExample {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            MyThread thread = new MyThread();
            thread.start(); // Starts a new thread
        }
    }
}
```



#### Key Differences
| Feature            | Multithreading 🧵 | Multiprocessing 🖥️ |
|--------------------|------------------|------------------|
| Execution Units   | Multiple **threads** in one **process** | Multiple **processes** |
| Memory Usage     | **Shared** memory | **Separate** memory |
| Performance     | Faster for **light** tasks | Better for **heavy** tasks |
| Overhead        | Lower (less memory used) | Higher (more memory needed) |
| Example         | Web browser, Chat apps | Video rendering, AI training |
---

#### ✅ **How many Ways to Create a Thread in Java?**

#### 🔹 **1. Extending the `Thread` class**  
👉 Suitable when you **don’t need to extend another class**.

#### **Example:**
```java
class MyThread extends Thread { // Extending Thread
    public void run() { // Override run() method
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread(); // Create thread
        t1.start(); // Start thread execution
    }
}
```

🟢 **Key Points:**
- `MyThread` **inherits** from `Thread`.
- `run()` contains the **task** for the thread.
- `start()` **calls `run()` internally in a separate thread**.

---

#### 🔹 **2. Implementing the `Runnable` interface**  
👉 Best when you **want to extend another class**.

#### **Example:**
```java
class MyRunnable implements Runnable { // Implementing Runnable
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable); // Pass to Thread constructor
        t1.start(); // Start thread
    }
}
```

🟢 **Key Points:**
- `MyRunnable` **implements** `Runnable` instead of `Thread`.
- We pass an **instance of `MyRunnable` to `Thread`**.
- `start()` runs the `run()` method **inside a new thread**.

---

#### 🔥 **Key Differences**
| Feature | Extending `Thread` | Implementing `Runnable` |
|---------|-----------------|-----------------|
| **Inheritance** | Can’t extend another class | Can extend another class |
| **Reusability** | Less reusable | More reusable |
| **Flexibility** | Less flexible | More flexible (recommended) |

👉 **Best Practice:** Prefer `Runnable` for better design and reusability.

---

#### 🔹 **When to Use Which?**
- **Use `Thread`** → When your class is not extending anything else.
- **Use `Runnable`** → When your class already extends another class (**Recommended for most cases**).

---

#### Why do we Prefer `Runnable` Over `Thread`?

#### 1. **Better Design (Separation of Concerns)**
   - `Runnable` follows **composition**, whereas `Thread` follows **inheritance**.
   - Java allows **only single inheritance**, so extending `Thread` prevents extending another class.

#### Example:
```java
// Implementing Runnable instead of extending Thread
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Running via Runnable");
    }
}

// Another class that can be extended since we use Runnable instead of Thread
class MyExtendedClass {} 

public class RunnableDesignExample {
    public static void main(String[] args) {
        // Create a Runnable instance
        MyTask task = new MyTask();
        
        // Pass it to a Thread instance
        Thread t = new Thread(task);
        
        // Start the thread
        t.start();
    }
}
```

#### 2. **Code Reusability**
   - If a class implements `Runnable`, it can be reused and passed to multiple threads.
   - A `Thread` object, once started, **cannot be restarted**, but a `Runnable` object can be reused.

#### Example:
```java
// Implementing Runnable so it can be used by multiple threads
class SharedTask implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " is executing the task");
    }
}

public class RunnableReusabilityExample {
    public static void main(String[] args) {
        // Creating a single instance of SharedTask
        SharedTask task = new SharedTask();
        
        // Creating multiple threads using the same Runnable instance
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        
        // Start both threads
        t1.start();
        t2.start();
    }
}
```

#### 3. **Resource Sharing**
   - Multiple threads can share the same `Runnable` instance, improving performance.
   - If extending `Thread`, a new instance is required for each new thread.

#### Example:
```java
class CounterTask implements Runnable {
    private int counter = 0; // Shared resource

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            counter++; // Incrementing shared counter
            System.out.println(Thread.currentThread().getName() + " Counter: " + counter);
        }
    }
}

public class RunnableSharingExample {
    public static void main(String[] args) {
        // Create a single instance of CounterTask
        CounterTask task = new CounterTask();
        
        // Multiple threads sharing the same task instance
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        
        // Start both threads
        t1.start();
        t2.start();
    }
}
```

#### 4. **Flexibility**
   - `Runnable` can be executed by `ThreadPoolExecutor`, `ScheduledExecutorService`, etc.
   - `Thread` is tightly coupled to the thread lifecycle, limiting flexibility.

#### Example:
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// A task that can be executed by multiple threads from a thread pool
class PooledTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task executed by " + Thread.currentThread().getName());
    }
}

public class RunnableFlexibilityExample {
    public static void main(String[] args) {
        // Creating a thread pool with 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Submitting multiple tasks to the thread pool
        for (int i = 0; i < 5; i++) {
            executor.execute(new PooledTask());
        }
        
        // Shutdown executor after task completion
        executor.shutdown();
    }
}
```

---

#### Example Code

#### Using `Runnable` (Preferred Approach)
```java
// Define a task by implementing Runnable interface
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task is running...");
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        // Create a Runnable instance
        MyTask task = new MyTask();
        
        // Create a Thread and pass Runnable instance to it
        Thread t1 = new Thread(task);
        
        // Start the thread
        t1.start();
    }
}
```

#### Using `Thread` (Less Preferred)
```java
// Define a task by extending Thread class
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        // Create an instance of MyThread
        MyThread t1 = new MyThread();
        
        // Start the thread
        t1.start();
    }
}
```

#### Conclusion:
✔ **Use `Runnable`** when the task is independent of the thread lifecycle.
❌ **Use `Thread`** only if you need to override its methods like `start()`, `join()`, etc.

---
#### Explain Thread Life Cycle in Java

A thread in Java goes through the following states during its execution:

1. **NEW**  
2. **RUNNABLE**  
3. **BLOCKED**  
4. **WAITING**  
5. **TIMED_WAITING**  
6. **TERMINATED**  

#### **1. NEW State**
- When a thread is created but **not yet started**, it is in the **NEW** state.  
- It remains in this state until `start()` is called.

#### **Example:**
```java
class NewStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        // Creating a thread but not starting it
        NewStateExample thread = new NewStateExample();
        System.out.println("Thread state: " + thread.getState()); // Output: NEW
    }
}
```

---

#### **2. RUNNABLE State**
- When `start()` is called, the thread moves from **NEW → RUNNABLE** state.
- It is now ready to run but waiting for CPU time.

#### **Example:**
```java
class RunnableStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        RunnableStateExample thread = new RunnableStateExample();
        thread.start(); // Now thread is in RUNNABLE state
        System.out.println("Thread state: " + thread.getState()); // Output: RUNNABLE or TERMINATED
    }
}
```

---

#### **3. BLOCKED State**
- A thread enters the **BLOCKED** state if it tries to access a **synchronized method** locked by another thread.
- It stays in this state until the lock is released.

#### **Example:**
```java
class BlockedStateExample {
    // Shared resource
    synchronized void sharedMethod() {
        System.out.println(Thread.currentThread().getName() + " is inside sharedMethod.");
        try {
            Thread.sleep(3000); // Simulating work
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class ThreadA extends Thread {
    BlockedStateExample obj;
    ThreadA(BlockedStateExample obj) { this.obj = obj; }

    public void run() {
        obj.sharedMethod();
    }
}

class ThreadB extends Thread {
    BlockedStateExample obj;
    ThreadB(BlockedStateExample obj) { this.obj = obj; }

    public void run() {
        obj.sharedMethod(); // Will be BLOCKED if ThreadA is inside this method
    }
}
```

---

#### **4. WAITING State**
- A thread goes into **WAITING** state when it calls `wait()`.
- It waits **indefinitely** until another thread calls `notify()`.

#### **Example:**
```java
class WaitingStateExample {
    public static void main(String[] args) throws InterruptedException {
        final Object lock = new Object();

        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread going into WAITING state...");
                    lock.wait(); // Thread enters WAITING state
                    System.out.println("Thread resumed after notify...");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        t1.start();
        Thread.sleep(1000);
        System.out.println("Thread state: " + t1.getState()); // Output: WAITING

        synchronized (lock) {
            lock.notify(); // Notify t1 to resume
        }
    }
}
```

---

#### **5. TIMED_WAITING State**
- A thread enters **TIMED_WAITING** state when it waits for a **fixed time** using:
  - `Thread.sleep(time)`
  - `wait(time)`
  - `join(time)`

#### **Example:**
```java
class TimedWaitingStateExample {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            try {
                System.out.println("Thread going into TIMED_WAITING state...");
                Thread.sleep(5000); // Thread enters TIMED_WAITING state
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        t1.start();
        Thread.sleep(1000);
        System.out.println("Thread state: " + t1.getState()); // Output: TIMED_WAITING
    }
}
```

---

#### **6. TERMINATED State**
- A thread moves to **TERMINATED** state after completing execution.

#### **Example:**
```java
class TerminatedStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) throws InterruptedException {
        TerminatedStateExample thread = new TerminatedStateExample();
        thread.start();
        Thread.sleep(1000); // Ensure thread execution is completed
        System.out.println("Thread state: " + thread.getState()); // Output: TERMINATED
    }
}
```

---

#### **Summary of Thread States**
| **State**           | **Description** |
|--------------------|----------------|
| **NEW**           | Thread created but not started. |
| **RUNNABLE**      | Thread started and waiting for CPU. |
| **BLOCKED**       | Thread waiting for a locked resource. |
| **WAITING**       | Thread waiting indefinitely for another thread’s signal. |
| **TIMED_WAITING** | Thread waiting for a fixed time (`sleep()`, `wait(time)`). |
| **TERMINATED**    | Thread finished execution. |

---

#### **Conclusion**
1. **Threads start in the NEW state.**  
2. **Once started, they go to RUNNABLE.**  
3. **If waiting for a lock, they go to BLOCKED.**  
4. **If waiting indefinitely, they go to WAITING.**  
5. **If waiting for a fixed time, they go to TIMED_WAITING.**  
6. **When execution completes, they go to TERMINATED.**  
---

### What is a Synchronized Block?

A synchronized block is a section of code marked with the synchronized keyword, which requires a thread to acquire a lock (also called a monitor) before executing that code. Only one thread can hold the lock at a time, forcing other threads to wait until the lock is released.

Syntax :

```java
synchronized (object) {
    // Code to be synchronized
}

```
- **`object`:** This is the object whose lock the thread must acquire. It acts as the monitor. It can be an instance of any class (e.g., `this` for the current object) or even a specific object created for locking purposes.

- **Code block:** The code inside the curly braces `{}` is protected from concurrent execution.

---

**How It Works ?**

- When a thread encounters a synchronized block, it attempts to acquire the lock on the specified object.

- If the lock is available (no other thread holds it), the thread acquires the lock and executes the block.

- If the lock is already held by another thread, the requesting thread waits until the lock is released.

- Once the thread finishes executing the synchronized block, it releases the lock, allowing other waiting threads to proceed.

**Example :**
```java
public class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) { // Synchronize on the current object
            count++;
        }
    }

    public int getCount() {
        synchronized (this) {
            return count;
        }
    }
}
```
In this example:

- Multiple threads calling `increment()` or `getCount()` will not interfere with each other because the synchronized block ensures that only one thread can modify or read `count` at a time.

- The lock is on `this` (the `Counter` instance), so all synchronized blocks using `this` as the monitor are mutually exclusive.

### Key Points :

1. **Granularity**: Unlike synchronized methods (where the entire method is locked), synchronized blocks allow you to limit the scope of synchronization to only the critical section of code, improving performance by reducing the time a lock is held.

2. **Lock Object**: The object used as the lock must be shared among threads that need to coordinate access. Using different objects as locks will not enforce mutual exclusion.

3. **Reentrancy**: Java's synchronization is reentrant, meaning the same thread can acquire the same lock multiple times (e.g., calling a synchronized method from within another synchronized method using the same lock).

4. **Deadlock Risk**: Improper use of synchronized blocks (e.g., locking multiple objects in different orders across threads) can lead to deadlocks, where threads wait indefinitely for each other to release locks.

---

### One Method with Single Custom Lock
```java
public class SharedResource {
    private int value = 0;
    private final Object lock = new Object(); // Dedicated lock object

    public void updateValue() {
        synchronized (lock) {
            value++;
            System.out.println(Thread.currentThread().getName() + " updated value to " + value);
        }
    }
}

```
Here, `lock` is a separate object used as the `synchronization` monitor instead of this (the `SharedResource` instance itself).

### What’s Happening?

- In Java, the `synchronized` keyword requires a lock (monitor) to control access to a block of code.

- You can use any object as a lock. Commonly, people use `this` (the current object), but here, a dedicated `Object lock` is created specifically for synchronization.

- Threads must acquire the `lock` object’s monitor to enter the `synchronized` (`lock`) block, ensuring only one thread updates `value` at a time.


---

### Two methods with single Custom Lock Examples :

### 🧸 Shared ToyBox Example in Java 
This example demonstrates how **two methods in a class use a single custom lock**. It means both methods share the same lock, and only **one thread can execute either method at a time**.

---

#### 🚀 Example Code: Shared Toy Box

```java
public class ToyBox {
    private int balls = 0;         // Number of balls in the box
    private int cars = 0;          // Number of toy cars in the box
    private final Object lock = new Object(); // One lock for both

    // Method 1: Add a ball to the box
    public void addBall() {
        synchronized (lock) {
            balls = balls + 1;
            System.out.println("Added a ball. Total balls: " + balls);
        }
    }

    // Method 2: Add a car to the box
    public void addCar() {
        synchronized (lock) {
            cars = cars + 1;
            System.out.println("Added a car. Total cars: " + cars);
        }
    }

    // Test it out
    public static void main(String[] args) {
        ToyBox box = new ToyBox();

        // Kid 1 adds balls
        Thread kid1 = new Thread(() -> {
            box.addBall();
            box.addBall();
        }, "Kid-1");

        // Kid 2 adds cars
        Thread kid2 = new Thread(() -> {
            box.addCar();
            box.addCar();
        }, "Kid-2");

        kid1.start();
        kid2.start();
    }
}
```

---

#### 🧠 Step-by-Step Explanation for Beginners

#### Step 1: What’s in the Code?

**Resources:**
- `balls`: Counts balls in the toy box (starts at 0).
- `cars`: Counts toy cars in the toy box (starts at 0).

**Lock:**
- `lock`: One key (lock) used for both `addBall()` and `addCar()`.

**Methods:**
- `addBall()`: Adds 1 ball, protected by `lock`.
- `addCar()`: Adds 1 car, protected by the same `lock`.

---

#### Step 2: How Does `addBall()` Work?

- A kid (thread like Kid-1) tries to add a ball.
- Kid-1 checks the `lock`:
  - If free, Kid-1 takes the lock and enters the block.
  - If taken, Kid-1 waits.
- Inside the block, it increments `balls` and prints.
- Then, Kid-1 releases the lock.

---

#### Step 3: How Does `addCar()` Work?

- Another kid (Kid-2) tries to add a car.
- Kid-2 checks the **same lock**:
  - If free, Kid-2 enters.
  - If not, Kid-2 waits.
- Inside, it increments `cars` and prints.
- Then, it releases the lock.

---

#### Step 4: Two Kids with One Lock

- **Kid-1** and **Kid-2** both need the same lock.
- They **cannot run at the same time**.
- If Kid-1 is adding balls, Kid-2 must **wait** until Kid-1 finishes.

---

#### Step 5: What You Might See (Output)

Possible outputs:

```
Added a ball. Total balls: 1
Added a ball. Total balls: 2
Added a car. Total cars: 1
Added a car. Total cars: 2
```

OR

```
Added a car. Total cars: 1
Added a car. Total cars: 2
Added a ball. Total balls: 1
Added a ball. Total balls: 2
```

Notice: The output is **not mixed**. One kid finishes all their actions before the other starts.

---

#### Step 6: Waiting in Action

- If **Kid-1** is inside `addBall()`, holding the lock:
  - **Kid-2** tries `addCar()`, sees the lock is taken, and waits.
- Only one can enter the toy box at a time!

---

#### 🔐 Why One Lock?

- **Like a Single Door**: Only one person (thread) can enter the room (critical section).
- **Why?** To **avoid conflicts** or keep things **safe**.

#### When to Use One Lock?

✅ Use **One Lock** If:
- Both actions must be done safely (no overlap).
- You want to avoid race conditions.

❌ Use **Two Locks** If:
- Actions are unrelated (can happen at the same time).
- Example: Updating two independent counters.

---

### Two Methods with two custom Locks Example :

```java
public class BankAccount {
    private int savings = 0;         // Money in savings
    private int checking = 0;        // Money in checking
    private final Object savingsLock = new Object();  // Lock for savings
    private final Object checkingLock = new Object(); // Lock for checking

    // Method 1: Deposit money into savings
    public void depositSavings(int amount) {
        synchronized (savingsLock) {
            savings = savings + amount;
            System.out.println("Deposited " + amount + " to savings. Total: " + savings);
        }
    }

    // Method 2: Deposit money into checking
    public void depositChecking(int amount) {
        synchronized (checkingLock) {
            checking = checking + amount;
            System.out.println("Deposited " + amount + " to checking. Total: " + checking);
        }
    }

    // Test it out
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        // Person 1 deposits to savings
        Thread person1 = new Thread(() -> {
            account.depositSavings(10);
            account.depositSavings(20);
        }, "Person-1");

        // Person 2 deposits to checking
        Thread person2 = new Thread(() -> {
            account.depositChecking(5);
            account.depositChecking(15);
        }, "Person-2");

        person1.start();
        person2.start();
    }
}

```
### Step-by-Step Explanation

**Step 1: What’s in the Code?**

- **Resources:**
    - `savings`: Tracks money in the savings account (starts at 0).

    - `checking`: Tracks money in the checking account (starts at 0).

- **Locks:**
    - `savingsLock`: A key just for the savings account.

    - `checkingLock`: A different key just for the checking account.

- **Methods:**
    - `depositSavings()`: Adds money to `savings`, protected by `savingsLock`.

    - `depositChecking()`: Adds money to `checking`, protected by `checkingLock`.

---

**Step 2: How Does `depositSavings()` Work?**
1. A person (thread, like Person-1) wants to deposit money into savings.

2. Person-1 checks the `savingsLock`:
    - If no one has it, Person-1 takes it and enters the `synchronized (savingsLock) block`.

    - If someone else has it, Person-1 waits.

3. Inside, Person-1 adds the amount to `savings` (e.g., `savings` goes from 0 to 10) and prints a message.

4. When finished, Person-1 leaves the block and gives back the `savingsLock`.

---
**Step 3: How Does depositChecking() Work?**
1. Another person (thread, like Person-2) wants to deposit money into checking.

2. Person-2 checks the `checkingLock`:
    - If no one has it, Person-2 takes it and enters the `synchronized (checkingLock)` block.

    - If someone else has it, Person-2 waits.

3. Inside, Person-2 adds the amount to `checking` (e.g., `checking` goes from 0 to 5) and prints a message.

4. When finished, Person-2 gives back the `checkingLock`.

---

**Step 4: Two People at Once**
- **Person-1** runs `depositSavings()` and needs the `savingsLock`.

- **Person-2** runs `depositChecking()` and needs the `checkingLock`.

- **Key Idea :** Since `savingsLock` and `checkingLock` are different, Person-1 and Person-2 can deposit money at the same time! Savings and checking are separate, so they don’t need to wait for each other.

---

**Step 5: What You Might See (Output)**
Running the `main` method could show:

```
Deposited 10 to savings. Total: 10
Deposited 5 to checking. Total: 5
Deposited 20 to savings. Total: 30
Deposited 15 to checking. Total: 20
```
The lines might mix because Person-1 and Person-2 work together. That’s fine—each person only changes their own account (`savings` or `checking`), and the locks keep it safe.

---

***Step 6: What If Two People Want Savings?**
- If Person-1 is in `depositSavings()` (holding `savingsLock`) and Person-3 tries `depositSavings()`:
    - Person-3 waits because Person-1 has the  `savingsLock`.

    - When Person-1 finishes, Person-3 gets the `savingsLock` and deposits money.

- Same idea for `checkingLock` in `depositChecking()`.

---

### One Method with One `this` Lock Concept


### 🍪 CookieJar Example - Understanding `this` as a Lock in Java

This simple example demonstrates how synchronization using `this` works in Java. We use a fun, beginner-friendly CookieJar scenario where multiple threads (kids) try to add cookies to the same jar. By using `synchronized(this)`, we ensure thread safety—only one kid can add a cookie at a time.

---

#### ✅ Example Code: Cookie Jar

```java
public class CookieJar {
    private int cookies = 0; // Number of cookies in the jar

    // Method: Add a cookie to the jar, synchronized with 'this'
    public void addCookie() {
        synchronized (this) {
            cookies = cookies + 1;
            System.out.println(Thread.currentThread().getName() + " added a cookie. Total: " + cookies);
        }
    }

    // Test it out
    public static void main(String[] args) {
        CookieJar jar = new CookieJar();

        // Kid 1 adds cookies
        Thread kid1 = new Thread(() -> {
            jar.addCookie();
            jar.addCookie();
        }, "Kid-1");

        // Kid 2 adds cookies
        Thread kid2 = new Thread(() -> {
            jar.addCookie();
            jar.addCookie();
        }, "Kid-2");

        kid1.start();
        kid2.start();
    }
}
```

---

#### 🧠 Step-by-Step Explanation for Beginners

#### 🧩 Step 1: What’s in the Code?

**Resource:**  
- `cookies`: Counts how many cookies are in the jar (starts at 0).

**Lock:**  
- `this`: The lock is the entire `CookieJar` object.

**Method:**  
- `addCookie()`: Adds 1 cookie, protected using `synchronized(this)`.

---

#### 🔐 Step 2: How Does `addCookie()` Work?

1. A kid (thread like `Kid-1`) wants to add a cookie.
2. `Kid-1` checks the lock, which is `this` (the `CookieJar` object).
3. If the lock is free, `Kid-1` enters the synchronized block.
4. Inside, the cookie count increases and a message is printed.
5. After finishing, `Kid-1` exits the block and releases the lock.

---

#### 🤼 Step 3: Two Kids Using the Same Method

- `Kid-1` and `Kid-2` both call `addCookie()`.
- Since both use the same lock (`this`), only **one** can add a cookie at a time.
- If one thread is inside the method, the other waits.

---

#### 🖨️ Step 4: Possible Output

You may see either of the following (but **not** mixed lines):

```
Kid-1 added a cookie. Total: 1  
Kid-1 added a cookie. Total: 2  
Kid-2 added a cookie. Total: 3  
Kid-2 added a cookie. Total: 4
```

**OR**

```
Kid-2 added a cookie. Total: 1  
Kid-2 added a cookie. Total: 2  
Kid-1 added a cookie. Total: 3  
Kid-1 added a cookie. Total: 4
```

🔒 Why no mixed lines? Because `this` is shared and only one thread can access the method at a time.

---

#### ⏳ Step 5: Waiting in Action

If `Kid-1` is in `addCookie()`, `Kid-2` waits.  
Only after `Kid-1` exits does `Kid-2` enter.  
It’s like there’s one key to the jar—the key **is the jar itself**.

---

#### 💡 Why Use `this`?

- Simple: No need for an extra lock object.
- Effective: Ensures only one thread accesses the critical section.
- Clear: Makes it obvious the lock is tied to the object itself.

---

#### 🧪 What If There Were Two Methods?

If you add another method like `removeCookie()` with `synchronized(this)`, it **also** uses the same lock (`this`).  
So `addCookie()` and `removeCookie()` **cannot** run at the same time either—they’ll take turns.

---

### Two method with one `this` Lock :

### Pet Store Synchronization Example in Java
---

#### Example Code: Pet Store
```java
public class PetStore {
    private int dogs = 0;    // Number of dogs in the store
    private int cats = 0;    // Number of cats in the store

    // Method 1: Add a dog to the store
    public void addDog() {
        synchronized (this) {
            dogs = dogs + 1;
            System.out.println(Thread.currentThread().getName() + " added a dog. Total dogs: " + dogs);
        }
    }

    // Method 2: Add a cat to the store
    public void addCat() {
        synchronized (this) {
            cats = cats + 1;
            System.out.println(Thread.currentThread().getName() + " added a cat. Total cats: " + cats);
        }
    }

    // Test it out
    public static void main(String[] args) {
        PetStore store = new PetStore();

        // Worker 1 adds dogs
        Thread worker1 = new Thread(() -> {
            store.addDog();
            store.addDog();
        }, "Worker-1");

        // Worker 2 adds cats
        Thread worker2 = new Thread(() -> {
            store.addCat();
            store.addCat();
        }, "Worker-2");

        worker1.start();
        worker2.start();
    }
}
```

---

#### Step-by-Step Explanation for Beginners

#### Step 1: What’s in the Code?
**Resources:**
- `dogs`: Counts dogs in the store (starts at 0).
- `cats`: Counts cats in the store (starts at 0).

**Lock:**
- `this`: The lock is the PetStore object itself. Both methods use this as their key.

**Methods:**
- `addDog()`: Adds 1 dog, protected by `this`.
- `addCat()`: Adds 1 cat, also protected by `this`.

---

#### Step 2: How Does `addDog()` Work?
- A worker (thread, like Worker-1) wants to add a dog.
- Worker-1 checks the lock (`this`, the PetStore object):
  - If no one has it, Worker-1 takes it and enters the `synchronized (this)` block.
  - If someone else has it, Worker-1 waits.
- Inside, Worker-1 adds 1 to `dogs` and prints a message.
- When done, Worker-1 releases the lock.

#### Step 3: How Does `addCat()` Work?
- Another worker (Worker-2) wants to add a cat.
- Worker-2 checks the same lock (`this`):
  - If free, takes it and enters.
  - If not, waits.
- Inside, Worker-2 adds 1 to `cats` and prints a message.
- Then releases the lock.

---

#### Step 4: Two Workers with One Lock
- Worker-1 calls `addDog()` and needs the lock (`this`).
- Worker-2 calls `addCat()` and also needs the lock (`this`).
- **Key Idea:** Since both use the same lock (`this`), only one can run at a time.

---

#### Step 5: Sample Output
```
Worker-1 added a dog. Total dogs: 1
Worker-1 added a dog. Total dogs: 2
Worker-2 added a cat. Total cats: 1
Worker-2 added a cat. Total cats: 2
```
OR
```
Worker-2 added a cat. Total cats: 1
Worker-2 added a cat. Total cats: 2
Worker-1 added a dog. Total dogs: 1
Worker-1 added a dog. Total dogs: 2
```

**Why not mixed?** One thread completes both actions before the other starts, since they share the same lock.

---

#### Step 6: Waiting in Action
- If Worker-1 is in `addDog()` (holding `this`):
  - Worker-2 tries `addCat()` but sees `this` is taken and waits.
  - Worker-2 starts only after Worker-1 finishes.

It’s like one key for the store—only one worker can use it at a time.

---

#### Why Use `this` for Both?

**Like One Store Key:**
- `this` is the whole PetStore.
- When locked with `synchronized (this)`, no one else can enter synchronized blocks.

**Compared to Two Locks:**
- With two separate locks (e.g., `dogLock` and `catLock`), both workers could run at the same time.
- Using `this` makes them take turns.

---

## Synchronized Methods

### Non-Static Synchronized Methods

 **Thumb Rule**
***Different objects = different locks = no blocking between them***

---

 **Synchronized Method(Non-Static - Single Instance - two Threads)** :
```java
package com.seleniumexpress.java8.multithreading;

public class Counter {
	
	private int count = 0;
	
	public synchronized void increment()
	{
		count++;
		try {
			Thread.sleep(10000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " incremented count to: " + count);
	}
	public int getCount()
	{
		return count;
	}
	
	public static void main(String[] args) {
		
		// first instance
		Counter c1 = new Counter();
		
		Thread t1 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t1.start();
		
		Thread t2 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t2.start();
		
	}

}

```

- The problem can be modeled as finding all possible ways to arrange 5 increments by Thread-0 and 5 increments by Thread-1 in a sequence of 10 steps. This is a combinatorial problem where we choose 5 positions out of 10 for Thread-0 (the remaining 5 are for Thread-1), and the count increases sequentially. 

- The number of possible sequences is given by the binomial coefficient:  
  C(10, 5) = 10! / (5! × 5!) = 252

- This means there are 252 possible distinct output sequences. Listing all of them explicitly would be impractical here, but I can explain the range of possibilities and provide representative examples.

```java
Thread-0 incremented count to: 1
Thread-0 incremented count to: 2
Thread-0 incremented count to: 3
Thread-0 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-1 incremented count to: 7
Thread-1 incremented count to: 8
Thread-1 incremented count to: 9
Thread-1 incremented count to: 10
```
```java
Thread-0 incremented count to: 1
Thread-1 incremented count to: 2
Thread-0 incremented count to: 3
Thread-1 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-0 incremented count to: 7
Thread-1 incremented count to: 8
Thread-0 incremented count to: 9
Thread-1 incremented count to: 10
```
```java
Thread-0 incremented count to: 1
Thread-0 incremented count to: 2
Thread-1 incremented count to: 3
Thread-1 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-0 incremented count to: 7
Thread-0 incremented count to: 8
Thread-1 incremented count to: 9
Thread-1 incremented count to: 10
```
**Why So Many Possibilities?**
- The **synchronized method ensures that each increment is atomic**, but it doesn’t dictate which thread gets the lock next after it’s released. Between each increment() call, the lock is released, and either thread can acquire it.

- The Thread.sleep(1000) delays execution within each call but doesn’t affect lock release. After the method ends, the lock is free, and the scheduler decides which thread runs next.

- With 10 increments and 2 threads, any sequence where Thread-0 gets 5 and Thread-1 gets 5 is valid, leading to 252 combinations.
---

**Key Points to Address Your Doubt**


**1. Lock Release After Method Execution**:
You’re absolutely correct: a `synchronized` method releases its lock when the method completes execution. In your code, the lock on the `Counter` object (`c1`) is released every time increment() finishes one call. So, after `Thread-0` increments `count` to 1 and prints the message, the lock is indeed released.

**2. Why Doesn’t Thread-1 Jump In?**:
If the lock is released after each `increment()` call, why doesn’t `Thread-1` get a chance to acquire it between `Thread-0’`s iterations? The answer lies in thread scheduling and the timing of execution, influenced by the `Thread.sleep(1000)` inside the `synchronized` method.


**3. What Happens Step-by-Step**:
Let’s walk through the execution:
1. `Thread-0 `starts and calls `c1.increment()`.

2. It acquires the lock on `c1`, increments `count` to 1, sleeps for 1 second (still holding the lock), prints "`Thread-0` incremented `count` to: 1", and then exits `increment()`.

3. At this point, the lock on `c1` is released because the method has completed.

4. `Thread-0` immediately proceeds to the next iteration of its for loop (since it’s still running and the loop is tight) and calls `c1.increment()` again, re-acquiring the lock.

5. Meanwhile, `Thread-1` is waiting to acquire the lock but doesn’t get a chance because Thread-0 is quick to re-acquire it after each release.


- The critical factor here is that Thread-0 doesn’t pause long enough between iterations for the JVM scheduler to give Thread-1 a chance to run. After releasing the lock, Thread-0 is still the active thread and jumps right back into the next iteration.

**4. Effect of Thread.sleep(1000)**:
The `sleep(1000)` inside `increment()` makes the thread pause for 1 second while holding the lock. However, once the method completes and the lock is released, there’s no delay before `Thread-0` moves to the next loop iteration. The transition from releasing the lock to re-acquiring it happens almost instantly (in CPU terms), leaving little opportunity for `Thread-1` to intervene.

During the 1-second sleep, `Thread-1` can’t do anything because `Thread-0` still holds the lock. After the sleep, when the lock is released, `Thread-0` is already poised to grab it again.

**5. Thread Scheduling and Fairness**:
Java’s thread scheduler doesn’t guarantee fairness by default. Once `Thread-0` starts running, it can keep executing its loop iterations back-to-back, re-acquiring the lock each time before `Thread-1` gets scheduled. The scheduler might not preempt `Thread-0` to let `Thread-1` run unless forced (e.g., by a higher-priority thread or explicit yielding).

In practice, `Thread-0` finishing all 5 iterations before `Thread-1` starts is a common outcome in this scenario, but it’s not strictly guaranteed—it’s a race condition influenced by scheduling.

**6. Why the Output Is Sequential**
Because `Thread-0` re-acquires the lock immediately after each `increment()` call, it completes all 5 iterations (count from 1 to 5) before `Thread-1` gets its turn. Only after `Thread-0` finishes its entire for loop and stops calling `increment()` does `Thread-1` acquire the lock and start its 5 iterations (count from 6 to 10).

The synchronized keyword ensures that each individual `increment()` call is atomic (no interleaving of increments), but it doesn’t enforce that the two threads take turns—it only ensures mutual exclusion.

**7. Could It Interleave?**
Yes, it’s theoretically possible for `Thread-1` to acquire the lock between two of `Thread-0`’s iterations if the scheduler decides to switch threads at just the right moment (e.g., after `Thread-0` releases the lock and before it re-acquires it). For example, you might see:

```java
Thread-0 incremented count to: 1
Thread-1 incremented count to: 2
Thread-0 incremented count to: 3
```

However, this requires the scheduler to preempt `Thread-0` and give `Thread-1` a chance, which doesn’t happen reliably in this code due to the tight loop and the timing. To force interleaving, you could:
Add `Thread.yield()` after `increment()` to hint the scheduler to switch threads.

Use a mechanism like a Lock with fairness policies (e.g., ReentrantLock with fair=true).


#### **Conclusion**:

The lock is released after each `increment()` call completes, as per the rules of synchronized. However, `Thread-0` quickly re-acquires it for the next iteration because it’s still running and the loop is tight. The 1-second sleep delays execution within each call but doesn’t help Thread-1 get in between iterations since the lock is held during the sleep.

The sequential output (all `Thread-0` then all `Thread-1`) is a result of `Thread-0` dominating the lock acquisition due to scheduling behavior, not because the lock isn’t released—it is released, just not in a way that gives `Thread-1` a fair shot until `Thread-0`is done.

---

**Two instances Three Threads Examples**
***Different objects = different locks = no blocking between them***

---
```java

public class Counter {
	
	private int count = 0;
	
	public synchronized void increment()
	{
		System.out.println("Lock Acquired by " + Thread.currentThread().getName());
		count++;
		try {
			Thread.sleep(100);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " incremented count to: " + count);
		System.out.println("Lock Released by " + Thread.currentThread().getName());
	}
	public int getCount()
	{
		return count;
	}
	
	public static void main(String[] args) {
		
		// first instance
		Counter c1 = new Counter();
		
		Thread t1 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t1.start();
		
		Thread t2 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t2.start();
		
		// first instance
		Counter c2 = new Counter();
		Thread t3 = new Thread(() -> 
		{
			for (int i = 0; i < 5; i++) {
				c2.increment();
			}	
		});
		t3.start();
		
	}

}

```

**output :**

```java
Lock Acquired by Thread-0
Lock Acquired by Thread-2
Thread-2 incremented count to: 1
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 1
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 2
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 2
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 3
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 3
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 4
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 4
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 5
Lock Released by Thread-2
Thread-0 incremented count to: 5
Lock Released by Thread-0
Lock Acquired by Thread-1
Thread-1 incremented count to: 6
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 7
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 8
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 9
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 10
Lock Released by Thread-1
```

**Output Explanation**

`Threads 1 and 2`: Both operate on `counter1`. Because `increment()` is `synchronized`, only one `thread` can execute it on `counter1` at a time. The output will show count incrementing from 0 to 10 in a thread-safe order (e.g., "`Thread-1` incremented count to 1", "`Thread-2` incremented count to 2", etc.).

`Thread 3`: Operates on `counter2`, a different instance. It runs concurrently with Threads 1 and 2 because `counter2` has its own lock. Its count goes from 0 to 5 independently.

Key Point
The `lock` is on the instance (counter1 or counter2), not the class. Different objects = different locks = no blocking between them.

---

