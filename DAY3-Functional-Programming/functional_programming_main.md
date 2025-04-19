
# Functional Programming in Java (Day 3) – Advanced Guide

## 🔹 1. **Lambda Expressions – Advanced Usage**

### ✅ Real-world Use Cases:
- Event listeners in GUIs
- Custom sorting logic in `Collections.sort`
- Simplifying multi-threading with `Runnable`
- Stream processing with concise syntax

### 🔍 Under the Hood:
- Lambdas are compiled to **invokedynamic** bytecode instructions.
- They don’t create anonymous classes; they are more efficient and *lazily loaded*.

### 💡 Advanced Tips:
- Use method references where possible: `String::toUpperCase` instead of `s -> s.toUpperCase()`.
- Combine lambdas for clarity: `predicate1.and(predicate2)`, `consumer1.andThen(consumer2)`.

---

## 🔹 2. **Streams API – Deep Dive & Internal Mechanics**

### 📌 Stream Creation:
- From `Collection.stream()`
- `Stream.of(...)`, `Arrays.stream(...)`
- Infinite streams: `Stream.iterate(...)`, `Stream.generate(...)`

### 🛠️ Intermediate Operations (lazy):
| Operation | Purpose |
|----------|---------|
| `map()` | Transform data |
| `filter()` | Select items that match a condition |
| `flatMap()` | Flatten nested structures (e.g., lists of lists) |
| `distinct()` | Remove duplicates |
| `sorted()` | Sort elements |
| `peek()` | Debug/log stream pipeline |

### ⚙️ Terminal Operations (eager):
| Operation | Purpose |
|----------|---------|
| `collect()` | Collect results |
| `reduce()` | Combine elements |
| `forEach()` | Execute an action |
| `anyMatch()`, `allMatch()` | Predicate checking |

### ⚠️ Performance Notes:
- Streams are lazy — intermediate operations do nothing until a terminal operation is invoked.
- Avoid mixing parallel and sequential streams arbitrarily.
- Avoid using stateful lambdas inside parallel streams (can lead to bugs).

---

## 🔹 3. **Collectors and Reduction – Internal Logic & Use**

### 🎯 Collectors:
Collectors allow us to *accumulate* elements into containers or other data structures.

```java
Map<String, List<Employee>> deptWise = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment));
```

### 🧮 Reduction:
Reduction is a terminal operation that aggregates stream elements into a single result.

#### Examples:
```java
// Sum using reduce
int total = List.of(1, 2, 3).stream()
              .reduce(0, (a, b) -> a + b);
```

### 📦 Types of Reductions:
- **Simple reduction**: `.reduce(0, Integer::sum)`
- **Reduction via collector**: `Collectors.reducing(...)`

#### Advanced Reducing:
```java
Optional<String> longest = strings.stream()
    .reduce((s1, s2) -> s1.length() > s2.length() ? s1 : s2);
```

---

## 🔹 4. **Functional Interfaces – Custom and Composable**

### 🎯 Built-in Interfaces:
| Interface | Method | Use |
|----------|--------|-----|
| `Function<T,R>` | `apply(T t)` | Transforms data |
| `Predicate<T>` | `test(T t)` | Filter logic |
| `Consumer<T>` | `accept(T t)` | Performs action |
| `Supplier<T>` | `get()` | Supplies values |
| `UnaryOperator<T>` | `apply(T t)` | One input/one output of same type |
| `BinaryOperator<T>` | `apply(T t1, T t2)` | Combine two inputs of same type |

### 🧩 Composing Functions:
```java
Function<String, String> trim = String::trim;
Function<String, String> toUpper = String::toUpperCase;
Function<String, String> combined = trim.andThen(toUpper);

System.out.println(combined.apply("  hello  ")); // "HELLO"
```

---

## 🔹 5. **Custom Collectors – Real Use Cases**

You can write a **custom collector** when `Collectors` doesn’t meet your need (e.g., custom aggregation, transformation).

### 🛠️ Collector Structure:
```java
Collector.of(
    () -> new StringBuilder(),          // Supplier: creates result container
    (sb, s) -> sb.append(s),            // Accumulator: adds to the result
    (sb1, sb2) -> sb1.append(sb2),      // Combiner: merges containers
    StringBuilder::toString             // Finisher: final transformation
);
```

### ✅ When to Use:
- Custom formatting (e.g., XML/JSON output)
- Domain-specific summaries
- Collecting into immutable or custom containers

---

## 🌐 Putting It All Together – Example

```java
Map<String, Double> averageSalaryByDept = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.averagingDouble(Employee::getSalary)
    ));
```

### Or even:
```java
List<String> topPerformers = employees.stream()
    .filter(e -> e.getRating() > 4)
    .map(Employee::getName)
    .sorted()
    .collect(Collectors.toList());
```

---

## 🧠 Bonus Concepts

- **Parallel Streams**: Use `.parallelStream()` to leverage multicore processing. But be cautious: thread-safety and order-sensitive logic can be problematic.
- **Short-Circuiting**: Operations like `anyMatch`, `findFirst` may terminate stream processing early.
- **Immutable Data & Side Effects**: Functional programming favors immutability. Avoid mutating shared state inside lambdas.

---

Would you like a **coding challenge**, a **real project scenario**, or a **cheat sheet** summarizing all these concepts?
