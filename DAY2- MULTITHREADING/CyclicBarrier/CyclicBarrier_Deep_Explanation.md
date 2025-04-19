
# 🔄 Understanding `CyclicBarrier` in Java

## ✅ What is CyclicBarrier?

`CyclicBarrier` is a synchronization aid in Java that lets a set of threads wait for each other at a common **barrier point** before continuing.

> 📌 Think of it like a checkpoint in a multiplayer game — everyone must reach it before moving on.

---

## 🧠 Key Concepts

- **Barrier Point**: Where threads pause and wait.
- **Parties**: Number of threads that must wait at the barrier.
- **Runnable Barrier Action (optional)**: Code run once all threads arrive.
- **Cyclic**: Can be **reused** after all threads pass the barrier.

---

## 🔧 How It Works

```java
CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All threads reached the barrier");
});
```

- All 3 threads must call `barrier.await()`
- Once the 3rd thread calls it, the **barrier action** runs
- Then all threads continue past the barrier

---

## 🔄 Why “Cyclic”?

Because it **resets automatically** after all threads pass it — perfect for repeated stages like game loops or simulations.

---

## 💡 Use Case

A race simulation:

- Racers prepare
- All wait at start line (barrier)
- Race begins when all ready
- Another barrier could be at the halfway point

---

## ✅ Full Example

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample {
    public static void main(String[] args) {
        int numberOfThreads = 3;

        CyclicBarrier barrier = new CyclicBarrier(numberOfThreads, () -> {
            System.out.println("✅ All threads reached the barrier – proceeding...");
        });

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

        for (int i = 0; i < numberOfThreads; i++) {
            new Thread(task, "Thread-" + i).start();
        }
    }
}
```

---

## 🕹️ Sample Output

```
Thread-1 is performing first part of the task.
Thread-0 is performing first part of the task.
Thread-2 is performing first part of the task.
Thread-1 is waiting at the barrier...
Thread-0 is waiting at the barrier...
Thread-2 is waiting at the barrier...
✅ All threads reached the barrier – proceeding...
Thread-0 continues after the barrier.
Thread-1 continues after the barrier.
Thread-2 continues after the barrier.
```

---

## 🔁 Multi-phase Simulation

Use inside a loop for repeated synchronization:

```java
for (int phase = 0; phase < 5; phase++) {
    System.out.println(name + " starting phase " + phase);
    barrier.await();
}
```

---

## 🆚 CyclicBarrier vs CountDownLatch

| Feature             | CyclicBarrier               | CountDownLatch              |
|---------------------|-----------------------------|------------------------------|
| Reusable?           | ✅ Yes (cyclic)              | ❌ No (one-time use)         |
| All threads wait?   | ✅ Yes (until all arrive)    | ❌ Only waiting threads wait |
| Runnable action?    | ✅ Yes                       | ❌ No built-in action        |

---

## ✅ Summary

- Use `CyclicBarrier` to **sync threads** at a **checkpoint**
- It’s **reusable** and perfect for **phased tasks**
- Great for **multiplayer games**, **AI simulation**, **scientific computation**

---

Let me know if you'd like diagrams, animations, or advanced usage examples!
