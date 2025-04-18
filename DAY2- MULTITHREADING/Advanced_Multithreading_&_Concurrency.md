
# 🧵 Day 2: Advanced Multithreading & Concurrency

## 🔸 1. ExecutorService, Callable, and Future

### 📌 ExecutorService
A high-level replacement for manually creating and managing threads.

Manages a pool of threads and allows submitting tasks without worrying about thread lifecycle.

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
executor.submit(() -> {
    System.out.println("Task executed by: " + Thread.currentThread().getName());
});
executor.shutdown();
```

**Common Implementations:**
- `Executors.newSingleThreadExecutor()` – One thread
- `Executors.newFixedThreadPool(n)` – Fixed number of threads
- `Executors.newCachedThreadPool()` – Reuses idle threads

### 📌 Callable & Future
`Callable<T>` is like `Runnable`, but it returns a result and can throw exceptions.

`Future<T>` represents the result of an asynchronous computation.

```java
Callable<Integer> task = () -> 10 + 20;
Future<Integer> result = executor.submit(task);
System.out.println(result.get()); // Blocks until result is available
```

## 🔸 2. Thread Safety, Race Conditions, and Deadlocks

### 📌 Thread Safety
Code is thread-safe if it works correctly when multiple threads access it concurrently.

Achieved via:
- synchronized blocks
- Locks (e.g., `ReentrantLock`)
- Atomic classes (`AtomicInteger`, `AtomicBoolean`)
- Thread-safe data structures (e.g., `ConcurrentHashMap`)

### 📌 Race Condition
Occurs when two threads access shared data and try to change it at the same time.

```java
int counter = 0;
void increment() {
    counter++; // Not thread-safe
}
```

**Fix with synchronized:**
```java
synchronized void increment() {
    counter++;
}
```

### 📌 Deadlocks
Happens when two or more threads are waiting indefinitely for locks held by each other.

```java
synchronized(lock1) {
    synchronized(lock2) {
        // do something
    }
}
```

If another thread does:

```java
synchronized(lock2) {
    synchronized(lock1) {
        // deadlock risk
    }
}
```

Prevent using consistent locking order or timeout locks.

## 🔸 3. ForkJoinPool

Designed for parallelism (breaking tasks into subtasks). Works with `RecursiveTask<T>` and `RecursiveAction`.

```java
ForkJoinPool pool = new ForkJoinPool();
int result = pool.invoke(new SumTask(1, 10000));
```

**Use case:** divide-and-conquer algorithms (e.g., merge sort, parallel sum).

## 🔸 4. CompletableFuture

A better, more flexible way to handle async programming. Supports non-blocking calls and chaining.

```java
CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenApply(result -> {
    return result + " World";
}).thenAccept(System.out::println);
```

**Useful methods:** `thenApply`, `thenAccept`, `thenCombine`, `exceptionally`, `handle`, `join`

## 🔸 5. CountDownLatch

Allows threads to wait until a set of operations in other threads completes.

```java
CountDownLatch latch = new CountDownLatch(3);
Runnable worker = () -> {
    // do some work
    latch.countDown(); // decrement count
};

for (int i = 0; i < 3; i++) {
    new Thread(worker).start();
}

latch.await(); // main thread waits here
System.out.println("All workers finished.");
```

## 🔸 6. CyclicBarrier

Allows a set of threads to all wait for each other to reach a common barrier point.

```java
CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All threads reached the barrier");
});

Runnable task = () -> {
    // perform part of task
    barrier.await(); // wait for others
    // continue after barrier
};
```

**Use case:** coordinating threads at a specific point (e.g., stage completion in a simulation).

## ✅ Summary Table

| Concept            | Purpose                        | Use Case                       |
|--------------------|--------------------------------|--------------------------------|
| ExecutorService    | Thread pool management         | Replace manual thread creation |
| Callable / Future  | Async task with return value   | Fetch result after execution   |
| Thread Safety      | Avoid data inconsistency       | Shared resource access         |
| Race Condition     | Data inconsistency due to concurrency | Unsynchronized counter  |
| Deadlock           | Threads wait forever for each other | Nested synchronized blocks |
| ForkJoinPool       | Divide tasks into subtasks     | Parallel computation           |
| CompletableFuture  | Functional async chaining      | APIs, reactive programming     |
| CountDownLatch     | One-time barrier               | Wait for multiple tasks        |
| CyclicBarrier      | Reusable barrier               | Synchronize multiple threads   |
