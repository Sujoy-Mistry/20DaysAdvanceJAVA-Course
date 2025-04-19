# 🧠 Functional Programming – Core Concepts Explained

## 📌 What Is Functional Programming?

Functional programming is a **programming paradigm** where **functions are first-class citizens**. It emphasizes:
- **Immutability**
- **Pure functions**
- **No side effects**
- **Declarative programming**

---

## 1. ✅ Pure Functions

**Definition:** A function that **always gives the same output** for the same input, with **no side effects**.

### 🧪 Example:
```js
const add = (a, b) => a + b;
```

- 🚫 No database calls
- 🚫 No API requests
- 🚫 No changes to external variables
- ✅ Just pure computation

💡 Pure functions are predictable and testable.

---

## 2. 🚫 No Side Effects

Functional programming avoids changing state or interacting with the outside world.

### ✅ OK:
```js
const square = x => x * x;
```

### ❌ Not OK:
```js
let result = 0;
const addToResult = x => result += x;
```

💡 Functional code is easier to reason about because it doesn’t rely on external changes.

---

## 3. 🧩 Immutability

Once a value is set, it does **not change**.

### ✅ OK:
```js
const arr = [1, 2, 3];
const newArr = [...arr, 4];
```

### ❌ Not OK:
```js
arr.push(4); // mutation
```

💡 Helps avoid bugs and keeps code predictable in concurrent environments.

---

## 4. 🛠️ Higher-Order Functions

Functions that **take functions as arguments** or **return functions**.

### ✅ Examples:
- `.map()`, `.filter()`, `.reduce()`
- Custom:
```js
const multiplyBy = x => y => x * y;
const double = multiplyBy(2);
console.log(double(4)); // 8
```

💡 Promotes abstraction and modularity.

---

## 5. 🔄 Function Composition

Build complex logic by **combining simpler functions**.

### ✅ Example:
```js
const toUpper = str => str.toUpperCase();
const exclaim = str => str + '!';
const shout = str => exclaim(toUpper(str));

shout("hello"); // "HELLO!"
```

💡 Encourages reuse and clean, readable pipelines.

---

## 6. 📋 Declarative Programming

You **describe what to do**, not **how to do it**.

### ✅ Declarative (Functional):
```js
const evenNumbers = numbers.filter(n => n % 2 === 0);
```

### ❌ Imperative:
```js
let evenNumbers = [];
for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % 2 === 0) evenNumbers.push(numbers[i]);
}
```

💡 Easier to understand and maintain.

---

## 7. ⚙️ Recursion Instead of Loops

Functional programming often replaces loops with **recursion**.

### ✅ Example:
```js
const factorial = n =>
  n === 0 ? 1 : n * factorial(n - 1);
```

💡 Especially powerful in languages where loops aren’t native (like Haskell).

---

## 8. 🧠 First-Class & Anonymous Functions

Functions can:
- Be assigned to variables
- Passed as arguments
- Returned from other functions

### ✅ Example:
```js
const greet = name => `Hello, ${name}`;
const sayHello = greet;
console.log(sayHello("John")); // Hello, John
```

💡 Treating functions like values = flexibility and power.

---

## 9. 🧼 Referential Transparency

An expression can be **replaced with its value** without changing the program’s behavior.

### ✅ Example:
```js
const a = 2 + 2;
const b = 4;
```

Both are interchangeable. This ensures **predictability** and makes refactoring safe.

---

## 📘 Summary Table

| Concept                  | Goal                                       | Example                         |
|--------------------------|--------------------------------------------|---------------------------------|
| Pure Functions           | No side effects                            | `x => x + 1`                    |
| Immutability             | Prevent state changes                      | `const newArr = [...arr]`      |
| Higher-Order Functions   | Abstract and reuse logic                   | `.map(fn)`                     |
| Function Composition     | Combine small functions into big ones      | `compose(fn1, fn2)`            |
| Declarative Style        | Focus on what, not how                     | `.filter(n => n > 0)`          |
| Recursion                | Replace loops                              | `factorial(n)`                 |
| First-Class Functions    | Pass functions around like values          | `const x = () => {...}`        |
| Referential Transparency | Replace expressions safely                 | `a = b + c` interchangeable    |

---

Let me know if you'd like:
- A visual timeline showing FP vs OOP styles
- Code examples in Java or Python
- Real-world problems solved using functional patterns
