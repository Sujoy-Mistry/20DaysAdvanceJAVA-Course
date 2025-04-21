# 🌱 Day 6: Dependency Injection with Spring Core

Since you have 2 years of experience, this guide offers a balanced explanation of **Dependency Injection (DI)** in Spring Core with practical and theoretical insight.

---

## 🔹 1. What is Dependency Injection (DI)?

Spring implements **Inversion of Control (IoC)** using **Dependency Injection**, where the control of object creation and wiring is transferred from the application code to the Spring container.

### ✅ Benefits:
- Decouples classes and their dependencies
- Makes unit testing easier
- Centralized configuration and object lifecycle management

### 🛠️ Types of DI in Spring:

#### 1. Constructor Injection
```java
@Component
public class Car {
    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

#### 2. Setter Injection
```java
@Component
public class Car {
    private Engine engine;

    @Autowired
    public void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

#### 3. Field Injection (not recommended for testing)
```java
@Component
public class Car {
    @Autowired
    private Engine engine;
}
```

---

## 🔹 2. Bean Lifecycle in Spring

Spring manages bean lifecycles automatically. You can hook into it with interfaces or annotations.

### 🔁 Lifecycle Phases:
1. Instantiation
2. Populate Properties
3. Set Bean Name & Factory
4. Pre-initialization (via `BeanPostProcessor`)
5. Custom init methods
6. Ready to Use
7. Destroy Phase (cleanup)

### 📌 Example Using Interfaces:
```java
@Component
public class MyBean implements InitializingBean, DisposableBean {

   @Override
   public void afterPropertiesSet() {
       System.out.println("Init logic here");
   }

   @Override
   public void destroy() {
       System.out.println("Cleanup logic here");
   }
}
```

### 📌 Example Using Annotations:
```java
@PostConstruct
public void init() { }

@PreDestroy
public void cleanup() { }
```

---

## 🔹 3. Annotations vs XML Configuration

| Feature               | Annotations                           | XML                                     |
|----------------------|----------------------------------------|-----------------------------------------|
| Configuration style  | Java-based (`@Component`, `@Bean`)    | XML (`<bean>` tags)                     |
| Maintainability      | Easier, closer to code                | Harder, external                        |
| Use case             | Preferred for modern apps             | Legacy projects, external config needed |
| Example              | `@Component`, `@Autowired`            | `<bean id="..." class="..."/>`          |

✅ **Best Practice**: Use annotations with Java config unless you’re maintaining legacy systems.

---

## 🔹 4. ApplicationContext and Bean Scopes

### 📦 ApplicationContext
Spring’s container responsible for:
- Creating and wiring beans
- Managing scope and lifecycle

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```

### 🧭 Bean Scopes:

| Scope      | Description                               |
|------------|-------------------------------------------|
| `singleton` (default) | One instance per Spring container  |
| `prototype`          | New instance every time requested  |
| `request`            | One per HTTP request (Web only)   |
| `session`            | One per HTTP session              |
| `application`        | One per ServletContext            |

#### 🔄 Scope Example:
```java
@Component
@Scope("prototype")
public class Task {
    // A new Task bean each time it's requested
}
```

---

## 📊 Visual Summary

![Dependency Injection Diagram](A_digital_diagram_illustrates_key_concepts_of_Depe.png)

---

## 💡 Want to go deeper?

- A hands-on project using constructor/setter DI?
- A GitHub template with both XML & annotation examples?
- A mini quiz or assignment for this module?

Let me know and I’ll hook you up! 🔧🚀
