
# 📘 Effective Java Best Practices (Detailed Guide)

A professional-level breakdown of selected best practices from *Effective Java*, enriched with explanations, examples, and real-world implications.

---

## ✅ Item 1: Consider Static Factory Methods Instead of Constructors

### 🔍 What It Means:
Use **static factory methods** instead of `new` constructors. Examples: `valueOf()`, `of()`, `from()`, `getInstance()`.

### ✅ Benefits:
- More descriptive names.
- Can return cached instances.
- Can return subtypes.
- Control instance creation.

### ✅ Example:
```java
public class Color {
    private final int red, green, blue;

    private Color(int r, int g, int b) {
        this.red = r;
        this.green = g;
        this.blue = b;
    }

    public static Color fromRGB(int r, int g, int b) {
        return new Color(r, g, b);
    }

    public static Color black() {
        return new Color(0, 0, 0);
    }
}
```

---

## ✅ Item 17: Minimize Mutability

### 🔍 What It Means:
Immutable objects cannot be changed after creation.

### ✅ Benefits:
- Thread-safe
- Easier to reason about
- Safe for caching and reuse

### ✅ How to Make Immutable:
- Declare class `final`
- Fields as `private final`
- No setters
- Use **defensive copies**

### ✅ Example:
```java
public final class Person {
    private final String name;
    private final Date birthDate;

    public Person(String name, Date birthDate) {
        this.name = name;
        this.birthDate = new Date(birthDate.getTime());
    }

    public String getName() {
        return name;
    }

    public Date getBirthDate() {
        return new Date(birthDate.getTime());
    }
}
```

---

## ✅ Item 18: Favor Composition Over Inheritance

### 🔍 What It Means:
Prefer **composition** (HAS-A) over inheritance (IS-A).

### ✅ Benefits:
- Loose coupling
- Better encapsulation
- More flexible

### ✅ Example:
```java
// Problem with inheritance
class InstrumentedList<E> extends ArrayList<E> {
    private int addCount = 0;

    @Override
    public boolean add(E e) {
        addCount++;
        return super.add(e);
    }
}

// Better with composition
class InstrumentedList<E> implements List<E> {
    private final List<E> list = new ArrayList<>();
    private int addCount = 0;

    public boolean add(E e) {
        addCount++;
        return list.add(e);
    }
}
```

---

## ✅ Item 50: Make Defensive Copies When Needed

### 🔍 What It Means:
Always copy **mutable objects** when accepting or returning them.

### ✅ Benefits:
- Protects internal state
- Preserves immutability

### ✅ Example:
```java
public class Period {
    private final Date start;
    private final Date end;

    public Period(Date start, Date end) {
        this.start = new Date(start.getTime());
        this.end = new Date(end.getTime());

        if (this.start.after(this.end)) {
            throw new IllegalArgumentException("Start date after end date");
        }
    }

    public Date getStart() {
        return new Date(start.getTime());
    }

    public Date getEnd() {
        return new Date(end.getTime());
    }
}
```

---

## ✅ Item 57: Minimize the Scope of Local Variables

### 🔍 What It Means:
Declare variables **close to usage** and avoid reusing.

### ✅ Benefits:
- Easier to read and debug
- Helps garbage collection

### ✅ Good Example:
```java
public void process() {
    for (int i = 0; i < 10; i++) {
        String message = "Value: " + i;
        System.out.println(message);
    }
}
```

### ❌ Bad Example:
```java
public void process() {
    String message = "";
    for (int i = 0; i < 10; i++) {
        message = "Value: " + i;
        System.out.println(message);
    }
}
```

---

Let me know if you want this as a slide deck or course handout!
