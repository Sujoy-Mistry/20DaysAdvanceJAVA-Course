
# 🧵 Why `main()` Doesn't Exit Immediately in Java (Thread Behavior Explained)

## ❓ The Confusion

You might notice that your `main()` method finishes executing, but the program **doesn’t terminate**. For example:

```java
public static void main(String[] args) {
    dummy dummy = new dummy();
    dummy.test();
    for(int i=0; i<4; i++){
        new Thread(dummy.task, "Thread" + i).start();
    }
    System.out.println("main thread done");
}
```

You see `"main thread done"` printed, yet the program stays alive. Why?

---

## 🔍 JVM Thread Lifecycle Rule

> The **JVM will stay alive** as long as **any non-daemon thread is running**.

Your worker threads (created using `new Thread(...)`) are **non-daemon by default**. This means:
- Even though `main()` has completed 
- The JVM **waits for all your threads to finish**

✅ So:
- `"hello"` prints ✅
- `main()` ends ✅
- JVM **waits** for all threads to finish ❗

---

## ✅ How to Confirm

You can add a print to the end of `main()`:

```java
System.out.println("main thread done");
```

You’ll see this prints before the other threads finish — confirming `main()` ended, but JVM didn’t shut down yet.

---

## 💥 Want `main()` to exit and kill other threads?

You have two options:

### 🔹 1. Set threads as Daemon

```java
Thread t = new Thread(dummy.task, "Thread" + i);
t.setDaemon(true);  // JVM will not wait for daemon threads
t.start();
```

> ⚠️ Daemon threads are killed when the main thread exits.

### 🔹 2. Use `ExecutorService` (Cleaner Approach)

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
for (int i = 0; i < 4; i++) {
    executor.submit(dummy.task);
}
executor.shutdown(); // JVM exits after all tasks finish
```

---

## 🧠 Summary

| Aspect                       | Behavior                  |
|-----------------------------|---------------------------|
| `main()` finishes            | ✅ Yes                    |
| JVM exits immediately        | ❌ No (non-daemon threads still alive) |
| Fix with daemon threads      | ✅ JVM will exit if only daemons remain |
| Fix with ExecutorService     | ✅ Recommended clean approach |

---

Let me know if you want a diagram or a full example using `ExecutorService`!
