
# 🧠 Spring Boot Auto-Configuration Discovery

Spring Boot simplifies application setup using **auto-configuration**. This is enabled by default via `@SpringBootApplication`, which includes `@EnableAutoConfiguration`.

But how does Spring Boot *discover* which configuration classes to load?

---

## 🔍 Class Discovery Mechanism

Spring Boot uses `META-INF` files to identify and load auto-configuration classes at runtime. The mechanism differs between Spring Boot 2.x and 3.x.

---

## 📦 Spring Boot 2.x and Earlier

### 📁 File:
```
META-INF/spring.factories
```

### 📄 Format:
```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.autoconfig.MyFeatureAutoConfiguration,org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

### ⚙️ Behavior:
- Spring Boot scans the classpath for all JARs.
- It looks for `spring.factories` inside each `META-INF/` folder.
- It reads the key `org.springframework.boot.autoconfigure.EnableAutoConfiguration`.
- All listed classes are treated as candidates for auto-configuration.
- These classes typically contain `@ConditionalOn...` annotations to control when they apply.

### ✅ Example:
```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.mycompany.autoconfig.MyCustomAutoConfiguration
```

---

## 🚀 Spring Boot 3.x and Newer

### 📁 File:
```
META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
```

### 📄 Format:
Plain list of fully-qualified class names, no keys.

```text
com.example.autoconfig.MyFeatureAutoConfiguration
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

### ⚙️ Behavior:
- Spring Boot reads this file at runtime to load listed classes.
- Classes are processed as auto-configuration candidates.
- Offers better startup performance and compatibility with native images.

---

## 🛠 Generating the File

Use the `spring-boot-autoconfigure-processor` to automatically generate the `.imports` file:

### 🔧 `pom.xml`:
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-autoconfigure-processor</artifactId>
  <optional>true</optional>
</dependency>
```

### 🏗 Example Configuration Class:
```java
@AutoConfiguration
@ConditionalOnClass(SomeService.class)
public class MyFeatureAutoConfiguration {
    @Bean
    public SomeService someService() {
        return new SomeService();
    }
}
```

This will automatically be included in:
```
META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
```

---

## 📊 Comparison Summary

| Version       | Discovery File                                                                 | Format      | Notes                                 |
|---------------|----------------------------------------------------------------------------------|-------------|----------------------------------------|
| Spring Boot 2 | `META-INF/spring.factories`                                                    | Key-Value   | Legacy format, still widely used       |
| Spring Boot 3 | `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` | Plain list | Faster, better native image support    |

---

## ✅ Best Practices

- Use `@AutoConfiguration` instead of `@Configuration` when creating auto-config classes (Spring Boot 3+).
- Prefer constructor binding and `@ConfigurationProperties` for clean config.
- Keep reusable logic in separate `*-autoconfigure` module and publish a lightweight starter.

---

## 🔬 Bonus Tip

Use Spring Boot Actuator's `/actuator/conditions` endpoint to debug why an auto-configuration class was or wasn’t applied.

---
