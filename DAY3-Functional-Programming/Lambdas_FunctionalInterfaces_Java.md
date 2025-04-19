
# ✅ Lambdas + Functional Interfaces = Dynamic & Clean Code

## 🔹 Functional Interface:
A **functional interface** is an interface with exactly **one abstract method** (can have default or static methods too).

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}
```

---

## 🔹 Implementing it the old way (Anonymous Inner Class):
```java
Greeting g = new Greeting() {
    public void sayHello(String name) {
        System.out.println("Hello, " + name);
    }
};
```

---

## 🔹 With a Lambda Expression (Anonymous Function):
```java
Greeting g = (name) -> System.out.println("Hello, " + name);
```

- Here, the lambda **(name) -> System.out.println(...)** is an **anonymous function**.
- Java knows you're assigning it to `Greeting`, which is a **functional interface**, so it uses that lambda to implement the abstract method `sayHello`.

---

## 🔥 What's Happening Internally?

Java uses a feature called **SAM conversion** (Single Abstract Method) to:
- Automatically wrap the lambda into an implementation of the interface.
- You don't have to write the boilerplate code yourself.

---

## 🧠 Summary:
| Concept | Explanation |
|--------|-------------|
| Lambda | Anonymous function |
| Functional Interface | Interface with one abstract method |
| Lambda + Functional Interface | Lambda automatically implements the interface method |

---

Would you like to try writing one? I can give you a mini coding challenge. Or I can show you how lambdas work with `Function`, `Predicate`, etc. Let me know!
