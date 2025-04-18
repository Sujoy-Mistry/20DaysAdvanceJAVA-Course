
# Java Multithreading Concepts: Thread Safety, Race Conditions, and Deadlocks

## 📌 Thread Safety

Thread safety refers to the property of a program or a part of it (usually functions, methods, or data structures) that guarantees it will function correctly when multiple threads access it concurrently.

### Key Methods for Achieving Thread Safety

#### 1. Synchronized Blocks
```java
public synchronized void increment() {
    counter++;
}
```
- **Pros**: Simple to use, guarantees mutual exclusion.
- **Cons**: Can cause performance issues if overused.

#### 2. Locks (e.g., ReentrantLock)
```java
ReentrantLock lock = new ReentrantLock();

public void increment() {
    lock.lock();
    try {
        counter++;
    } finally {
        lock.unlock();
    }
}
```
- **Pros**: More control, supports tryLock.
- **Cons**: Requires more code, needs careful handling.
![ChatGPT Image Apr 18, 2025, 09_54_52 PM](https://github.com/user-attachments/assets/78980122-9969-47cb-82f3-bdcb016a2c1b)


#### 3. Atomic Classes
```java
AtomicInteger counter = new AtomicInteger(0);

public void increment() {
    counter.incrementAndGet();
}
```
- **Pros**: Efficient, non-blocking.
- **Cons**: Limited to simple operations.

#### 4. Thread-Safe Data Structures
```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("key", 1);
```
- **Pros**: Convenient, built-in thread safety.
- **Cons**: May have performance overhead under high contention.

---

## 📌 Race Condition

Occurs when multiple threads access and modify shared data concurrently.

### Example:
```java
int counter = 0;

void increment() {
    counter++;  // Not thread-safe
}
```

### Fix with Synchronized:
```java
synchronized void increment() {
    counter++;
}
```

### Other Solutions:
- Use Locks (`ReentrantLock`)
- Use Atomic classes (`AtomicInteger`)

---

## 📌 Deadlocks

A **deadlock** occurs when two or more threads are blocked forever, waiting on each other to release locks.

### Example:
```java
// Thread 1:
synchronized(lock1) {
    synchronized(lock2) {
        // do something
    }
}

// Thread 2:
synchronized(lock2) {
    synchronized(lock1) {
        // do something
    }
}
```

### Prevention Strategies:

#### 1. Consistent Locking Order
```java
synchronized(lock1) {
    synchronized(lock2) {
        // safe
    }
}
```

#### 2. Timeout Locks
```java
if (lock1.tryLock(10, TimeUnit.SECONDS)) {
    try {
        if (lock2.tryLock(10, TimeUnit.SECONDS)) {
            try {
                // do something
            } finally {
                lock2.unlock();
            }
        }
    } finally {
        lock1.unlock();
    }
}
```

#### 3. Deadlock Detection
- Use monitoring tools or implement deadlock detection algorithms.

#### 4. Timeout for Entire Process
- Abort or retry the operation if it exceeds a certain duration.

---

## ✅ Conclusion

- Thread safety ensures correct behavior in multithreaded environments.
- Race conditions result from unsynchronized access to shared data.
- Deadlocks occur due to improper locking strategies.
- Prevent these issues with the right synchronization tools and design strategies.
