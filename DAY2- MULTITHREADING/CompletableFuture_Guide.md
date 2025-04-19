
# 🔹 CompletableFuture — Flexible Async Programming in Java

![Async Java](https://upload.wikimedia.org/wikipedia/commons/7/7e/Java_async_flow.png)

## ✅ What Is CompletableFuture?

`CompletableFuture` is a class in the `java.util.concurrent` package (added in Java 8) that allows you to write non-blocking asynchronous code. It helps run tasks in the background and chain multiple stages of computation in a readable and flexible way.

---

## 🔧 Basic Example

```java
CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenApply(result -> {
    return result + " World";
}).thenAccept(System.out::println);
```

### 🔍 Breakdown:

| Method          | Purpose                                                        |
|-----------------|----------------------------------------------------------------|
| `supplyAsync()` | Starts an asynchronous task that returns a result (`Supplier<T>`) |
| `thenApply()`   | Processes the result and returns transformed value (`Function<T, R>`) |
| `thenAccept()`  | Consumes the result, returns nothing (`Consumer<T>`)            |

✅ **Result**: Prints `Hello World`

---

## 📚 Common Methods in `CompletableFuture`

| Method           | Purpose                                                       |
|------------------|---------------------------------------------------------------|
| `supplyAsync()`  | Starts a background task with a return value                  |
| `runAsync()`     | Starts a background task without a return value               |
| `thenApply()`    | Transforms the result                                         |
| `thenAccept()`   | Consumes the result without returning                         |
| `thenRun()`      | Executes a task after another finishes, doesn’t access result |
| `thenCombine()`  | Combines two `CompletableFutures`                             |
| `handle()`       | Handles result or exception, returns a value                  |
| `exceptionally()`| Handles exceptions, returns a fallback                        |
| `join()`         | Blocks and returns result (unchecked exception)              |
| `allOf()`        | Waits for all provided futures to complete                    |
| `anyOf()`        | Returns as soon as one future completes                       |

---

## 🌐 Async Chaining in Action

```java
CompletableFuture.supplyAsync(() -> {
    return 10;
}).thenApply(x -> x * 2)
  .thenApply(x -> x + 5)
  .thenAccept(System.out::println); // prints 25
```

---

## 🔁 Combining Multiple Futures

```java
CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> 10);
CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> 20);

future1.thenCombine(future2, (a, b) -> a + b)
       .thenAccept(System.out::println); // prints 30
```

---

## 🧨 Exception Handling

### `exceptionally()`

```java
CompletableFuture.supplyAsync(() -> {
    if (true) throw new RuntimeException("Oops!");
    return "OK";
}).exceptionally(ex -> {
    System.out.println("Caught: " + ex);
    return "Fallback";
}).thenAccept(System.out::println); // prints "Fallback"
```

### `handle()`

```java
CompletableFuture.supplyAsync(() -> {
    return 10 / 0;
}).handle((res, ex) -> {
    if (ex != null) return 0;
    return res;
}).thenAccept(System.out::println); // prints 0
```

---

## 🚦 Blocking vs Non-Blocking

- ✅ **Non-blocking**: Methods like `thenApply`, `thenAccept`, etc., return immediately.
- ❌ **Blocking**: Methods like `get()` and `join()` block until result is ready. Use with caution!

---

## 🧵 Thread Management

By default, `CompletableFuture.supplyAsync()` uses the **ForkJoinPool.commonPool()**.

You can also provide your own `Executor`:

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
CompletableFuture.supplyAsync(() -> "Custom", executor)
                 .thenAccept(System.out::println);
```

---

## ✅ When to Use `CompletableFuture`

- Complex async workflows with chaining
- Running multiple tasks in parallel and combining results
- Handling timeouts and exceptions in async operations
- Non-blocking UI updates (GUI/Web)
- Replacing traditional callbacks ("callback hell")

---

## 🧠 Summary

| Feature                | Benefit                                       |
|------------------------|-----------------------------------------------|
| Chainable              | Write readable sequences of async steps       |
| Non-blocking           | Keeps threads free for other work             |
| Exception handling     | Built-in handling with `handle()` etc.        |
| Parallel execution     | Combine multiple async tasks easily           |
| Optional custom threads| Control over performance and resources        |
