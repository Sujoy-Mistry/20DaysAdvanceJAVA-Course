
# Can a Non-Static Class Have Static Methods in Java?

Yes – A **non-static (regular)** class **can have static methods**. This is **very common** in Java.

---

## 🔍 Why Is This Allowed?

In Java, **static methods belong to the class, not the instance**. So even if the class is not declared `static`, you can still define static methods inside it.

The only place where **you can’t have static methods** is inside **non-static inner classes**, because they belong to an instance of an outer class.

---

## ✅ Example: Static Method in a Regular Class

```java
public class Color {
    private final int r, g, b;

    private Color(int r, int g, int b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }

    // ✅ Static factory method in a regular class
    public static Color rgb(int r, int g, int b) {
        return new Color(r, g, b);
    }

    public String toString() {
        return "(" + r + "," + g + "," + b + ")";
    }
}
```

### Usage:
```java
Color green = Color.rgb(0, 255, 0); // ✅ No need to create an instance to call rgb()
System.out.println(green);         // prints: (0,255,0)
```

---

## ❌ Not Allowed: Static Method in a Non-Static Inner Class

```java
public class Outer {
    public class Inner {
        public static void hello() { } // ❌ Compilation error!
    }
}
```

You can’t declare static methods in a **non-static inner class**, because inner classes are tied to instances of the outer class, and static means "class-level", not "instance-level".

---

## ✅ Summary

| Class Type                 | Can Have Static Methods? | Notes                                     |
|---------------------------|---------------------------|-------------------------------------------|
| Regular class             | ✅ Yes                    | Most common case                         |
| Static nested class       | ✅ Yes                    | Behaves like a top-level class           |
| Non-static inner class    | ❌ No                     | Bound to an instance of the outer class  |

---

Let me know if you want a cheat sheet on class types and when to use `static`.
