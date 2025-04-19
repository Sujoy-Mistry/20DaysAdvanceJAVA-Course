
# ✅ Java Streams: `map()` vs `filter()` – Functional Interfaces Explained

> ❌ **No, `map()` does *not* take a `Predicate`.**  
> ✅ **`map()` takes a `Function`.**

---

## 🔍 Let’s break it down:

### ✅ `map()` – Used to **transform** elements
- It takes a `Function<T, R>` (input type `T`, returns type `R`)
- Example: convert a list of strings to their lengths

```java
List<String> words = List.of("apple", "banana", "kiwi");

List<Integer> lengths = words.stream()
    .map(word -> word.length())  // Function<String, Integer>
    .collect(Collectors.toList());
```

---

### ❓ So what's a **Predicate** used for?

### ✅ `filter()` – Used to **select** elements based on a condition
- It takes a `Predicate<T>` (input `T`, returns `boolean`)

```java
List<String> fruits = List.of("apple", "banana", "kiwi");

List<String> longFruits = fruits.stream()
    .filter(fruit -> fruit.length() > 5) // Predicate<String>
    .collect(Collectors.toList());
```

---

## ✅ Quick Summary:

| Method     | Interface it uses   | Purpose                                      |
|------------|---------------------|----------------------------------------------|
| `map()`    | `Function<T, R>`    | Transform data (e.g., convert String to Integer) |
| `filter()` | `Predicate<T>`      | Keep only items that match a condition       |
| `forEach()`| `Consumer<T>`       | Perform an action (e.g., print each item)    |

---

Let me know if you want to go deeper into how these functional interfaces are defined and used under the hood!
