
# 📘 Java Static Factory Methods: How to Use `valueOf()`, `of()`, `from()`, `getInstance()`, `newInstance()`

Java provides various static factory methods as alternatives to constructors. Here's a detailed guide on when and how to use them effectively.

---

## 🔧 1. `valueOf()`

### ✅ Purpose:
Returns a cached or canonical instance (commonly used for wrapper classes).

### 🔸 Example: `Boolean`, `Integer`
```java
Boolean bool = Boolean.valueOf(true);       // returns Boolean.TRUE
Integer num = Integer.valueOf(100);         // returns cached Integer if possible
```

> 💡 Behind the scenes, it may reuse existing objects (e.g., `Integer.valueOf(127)` reuses objects for performance).

---

## 🛠 2. `of()`

### ✅ Purpose:
Used for creating objects in a concise way.

### 🔸 Example: Java Collections (Immutable)
```java
List<String> names = List.of("Alice", "Bob", "Charlie");
Set<Integer> numbers = Set.of(1, 2, 3);
```

### 🔸 Custom Example:
```java
public class Point {
    private final int x, y;
    private Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public static Point of(int x, int y) {
        return new Point(x, y);
    }
}
Point p = Point.of(5, 10);
```

---

## 🔨 3. `from()`

### ✅ Purpose:
Often used for type conversion or transformation.

### 🔸 Example: Java Date-Time API
```java
DayOfWeek day = DayOfWeek.from(LocalDate.now());
```

### 🔸 Custom Example:
```java
public class Temperature {
    private final double celsius;

    private Temperature(double celsius) {
        this.celsius = celsius;
    }

    public static Temperature fromFahrenheit(double f) {
        return new Temperature((f - 32) * 5 / 9);
    }
}
Temperature t = Temperature.fromFahrenheit(98.6);
```

---

## 🧰 4. `getInstance()`

### ✅ Purpose:
Used in Singleton patterns or when instance creation needs to be controlled.

### 🔸 Example: `Calendar`, Singleton Pattern
```java
Calendar cal = Calendar.getInstance();

public class Config {
    private static final Config INSTANCE = new Config();
    private Config() {}

    public static Config getInstance() {
        return INSTANCE;
    }
}
Config c = Config.getInstance();
```

---

## 🛠️ 5. `newInstance()`

### ✅ Purpose:
Creates a new instance dynamically — often used in reflection.

### 🔸 Example: Reflection-based instantiation
```java
Class<?> clazz = Class.forName("java.util.ArrayList");
List list = (List) clazz.getDeclaredConstructor().newInstance();
```

> ⚠️ Caution: This is less safe and rarely used in modern Java unless writing frameworks or tools.

---

## 🎯 Summary Table

| Method         | Use Case                                  | Example Class               |
|----------------|--------------------------------------------|-----------------------------|
| `valueOf()`    | Caching, reuse, parsing                    | `Integer`, `Boolean`, `Enum` |
| `of()`         | Concise object creation                    | `List.of()`, custom factories |
| `from()`       | Conversion between types                   | `DayOfWeek.from()`, custom   |
| `getInstance()`| Singleton or controlled instance creation | `Calendar`, `Config`         |
| `newInstance()`| Reflection-based dynamic creation         | `Class.newInstance()`        |
