
# 🔍 Java `CyclicBarrier` Code Walkthrough

This document walks through a Java program step by step that demonstrates how to use `CyclicBarrier` to coordinate multiple threads.

---

## ✅ Imports

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
```

- **`CyclicBarrier`**: The main class for coordinating threads at a barrier point.
- **`BrokenBarrierException`**: Thrown if the barrier breaks while waiting.
- **`InterruptedException`**: Thrown if a thread is interrupted while waiting.

---

## 👇 Main Class and Method

```java
public class CyclicBarrierExample {
    public static void main(String[] args) {
```

Defines the main method which begins execution.

---

## 🔧 Step 1: Create a CyclicBarrier

```java
int numberOfThreads = 3;

CyclicBarrier barrier = new CyclicBarrier(numberOfThreads, () -> {
    System.out.println("✅ All threads reached the barrier – proceeding...");
});
```

- `numberOfThreads = 3`: Total threads to sync.
- `new CyclicBarrier(...)`: Constructs the barrier.
  - Number of threads that must call `await()` before proceeding.
  - A Runnable action executed once after the last thread arrives.

### Barrier Action

```java
() -> System.out.println("✅ All threads reached the barrier – proceeding...");
```

Runs once when all threads have reached the barrier.

---

## 🔄 Step 2: Define the Thread Task

```java
Runnable task = () -> {
    String name = Thread.currentThread().getName();
    try {
        System.out.println(name + " is performing first part of the task.");
        Thread.sleep((int)(Math.random() * 1000));

        System.out.println(name + " is waiting at the barrier...");
        barrier.await();

        System.out.println(name + " continues after the barrier.");
    } catch (InterruptedException | BrokenBarrierException e) {
        e.printStackTrace();
    }
};
```

### Thread Flow

1. 🛠 Perform some work (simulate with sleep).
2. ⏳ Wait at the barrier.
3. ✅ Resume once all threads reach the barrier.

---

## 🚀 Step 3: Start Threads

```java
for (int i = 0; i < numberOfThreads; i++) {
    new Thread(task, "Thread-" + i).start();
}
```

- Starts 3 threads running the same task.

---

## 🧾 Example Output

```
Thread-1 is performing first part of the task.
Thread-2 is performing first part of the task.
Thread-0 is performing first part of the task.
Thread-2 is waiting at the barrier...
Thread-1 is waiting at the barrier...
Thread-0 is waiting at the barrier...
✅ All threads reached the barrier – proceeding...
Thread-2 continues after the barrier.
Thread-1 continues after the barrier.
Thread-0 continues after the barrier.
```

Order may vary due to thread scheduling and sleep randomness.

---

## 🔄 Why Use `CyclicBarrier`?

### Real-World Applications

- **Games**: Wait for all players to be ready.
- **Simulations**: Synchronize at different stages.
- **Multi-stage Processing**: Pause before advancing to the next stage.

---

## 🧠 Summary

| Part                         | Purpose                                              |
|-----------------------------|------------------------------------------------------|
| `CyclicBarrier barrier`     | Synchronizes threads at a common point              |
| `barrier.await()`           | Causes thread to wait until all others arrive       |
| Runnable `task`             | Defines thread behavior                             |
| Barrier Runnable Action     | Runs once when all threads have reached the barrier |
| Threads loop                | Starts multiple threads to run the same task        |

---

Let me know if you want:
- A multi-phase simulation example
- A diagram to visualize the barrier mechanism
- A comparison with `CountDownLatch` or `Phaser`
