
# 🧠 Functional Programming in Java – Deep Dive with Streams

Functional Programming (FP) in Java is about writing declarative, concise, and side-effect-free code using **functions as first-class citizens**. Here's a comprehensive look at how it works with `map`, `filter`, `stream`, and more.

---

## 🔷 Core Concept: Functional Programming

- **Declarative style**: Focus on **what** to do rather than **how**.
- Emphasizes **immutability**, **pure functions**, and **data transformation**.
- Uses **functions** as inputs/outputs.

---

## 🔹 Data Source

Streams typically begin with a data source like:

- `Collection` (`List`, `Set`, etc.)
- `Array`
- Generated sequences (`Stream.generate`, `Stream.iterate`)

Example:
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
```

---

## 🔹 Stream Pipeline

### ➤ Stream Creation
```java
names.stream()
Stream.of(...)
Arrays.stream(...)
IntStream.range(1, 10)
```

---

### ➤ Intermediate Operations (Lazy)
These return another stream. They don’t trigger execution.

| Method | Purpose | Functional Interface |
|--------|---------|----------------------|
| `map()` | Transform data | `Function<T, R>` |
| `filter()` | Select items that match condition | `Predicate<T>` |
| `flatMap()` | Flatten nested structures | `Function<T, Stream<R>>` |
| `sorted()` | Sort elements | `Comparator<T>` |
| `distinct()` | Remove duplicates | — |
| `peek()` | Debug/log each element | `Consumer<T>` |

Example:
```java
names.stream()
     .filter(name -> name.startsWith("A"))
     .map(String::toUpperCase)
```

---

### ➤ Terminal Operations (Eager)
These trigger the actual computation.

| Method | Purpose | Functional Interface |
|--------|---------|----------------------|
| `collect()` | Aggregate result | Various `Collector` types |
| `reduce()` | Combine into one result | `BinaryOperator<T>` |
| `forEach()` | Perform action per item | `Consumer<T>` |
| `count()` | Count elements | — |
| `anyMatch()` | Predicate checking | `Predicate<T>` |
| `findFirst()` | Return first element | — |

Example:
```java
List<String> result = names.stream()
    .filter(n -> n.length() > 3)
    .collect(Collectors.toList());
```

---

## 🔹 Functional Interfaces – Under the Hood

| Interface | Purpose | Example |
|-----------|---------|---------|
| `Function<T, R>` | Transform input to output | `map()` |
| `Predicate<T>` | Test condition | `filter()` |
| `Consumer<T>` | Perform action | `forEach()` |
| `Supplier<T>` | Supply data | `Stream.generate()` |
| `BinaryOperator<T>` | Combine two values | `reduce()` |

---

## 🌀 Bonus Concepts

- **Immutability**: Data isn't modified; new values are returned.
- **Pure Functions**: Output depends only on input.
- **Side Effects**: Avoided in functional programming.
- **Parallel Streams**: Use `parallelStream()` for concurrency — but avoid shared mutable state.

---

This guide shows how Java functional programming empowers you to write cleaner, safer, and more expressive code. Let me know if you'd like a poster or printable version!
