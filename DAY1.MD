# 🔹 Java Memory Architecture: Breakdown of the JVM Structure

The Java Virtual Machine (JVM) uses different memory areas to manage and store data during program execution. These areas help the JVM manage memory efficiently and are critical for performance tuning, garbage collection, and application stability.

---

## 1. 🧠 Heap: Where Objects Are Stored

**Definition**: The heap is the area of memory where objects are dynamically allocated. All Java objects are created in the heap, and it is where garbage collection happens.

### 🔸 Role:
- The heap is the largest memory area in the JVM and can grow or shrink during the application's execution.
- Objects created using `new` (e.g., `new String()`) are stored in the heap.
- JVM automatically reclaims memory with **Garbage Collection (GC)** for unused objects.

### 📦 Memory Segmentation:
- **Young Generation**: Where newly created objects are allocated.
  - **Eden Space**: Initial allocation area for new objects.
  - **Survivor Spaces (S0, S1)**: Stores objects that survived GC cycles.
- **Old Generation**: Where long-lived objects are promoted after surviving multiple GC cycles.

---

## 2. 📚 Stack: Local Variables and Method Calls

**Definition**: Stack is used for the execution of threads — for method calls, local variables, and partial results.

### 🔸 Role:
- Each thread has its **own stack**.
- Every method call creates a **stack frame** with:
  - Local variables (including object references)
  - Return address
  - Partial results
- Follows **LIFO** (Last-In, First-Out) order.
- Stack memory is **limited**, and overuse (e.g., deep recursion) may result in `StackOverflowError`.

---

## 3. 🧾 Metaspace (Formerly PermGen)

**Definition**: Metaspace is the memory area used to store class metadata (introduced in Java 8, replacing PermGen).

### 🔸 Role:
- Stores structure and metadata of classes loaded by JVM.
- Does **not** store actual objects.
- Grows automatically (unlike PermGen which had a fixed size).
- Configurable with: `-XX:MaxMetaspaceSize`.

### 📌 Notes:
- OutOfMemoryError may occur due to class loader leaks or excessive class loading.

---

## 4. 🧭 Program Counter (PC)

**Definition**: The Program Counter (PC) is a small memory area in each thread that stores the address of the next instruction to be executed.

### 🔸 Role:
- Each thread has its own PC.
- Tracks the current or next executing instruction.
- For native methods (written in C/C++), PC is undefined.

---

## 5. 🧩 Memory Areas and Their Sizes

### 🟢 Young Generation
- **Definition**: Where new objects are initially allocated.
- **Components**:
  - **Eden Space**: First allocation zone.
  - **Survivor Spaces (S0/S1)**: Hold surviving objects.
- **Role**:
  - Optimized for short-lived objects.
  - Minor GC happens frequently here.
  - Survivors promoted to Old Generation.
- **Size Config**:
  - `-Xmn` or `-XX:NewSize`, `-XX:MaxNewSize`
  - Typically ~1/3 of total heap.

### 🔵 Old Generation (Tenured)
- **Definition**: Stores objects that have lived long enough in the Young Generation.
- **Role**:
  - Less frequent collection (Major GC).
  - Holds long-lived data like caches, states.
- **Size Config**:
  - `-XX:OldSize`, `-XX:MaxOldSize`
  - Typically ~2/3 of total heap.

### 🟡 Eden Space
- **Definition**: Sub-part of Young Generation where objects are first placed.
- **Role**:
  - Allocates memory for newly created objects.
  - Objects that survive are moved to Survivor spaces.

### 🟠 Survivor Spaces (S0, S1)
- **Definition**: Part of Young Generation used to store objects that survived GC in Eden.
- **Role**:
  - Objects are copied back and forth between S0 and S1.
  - Eventually, long-living objects get promoted to Old Gen.

---

## 6. ♻️ Garbage Collection (GC) Impact

### 🔹 Minor GC
- Occurs in Young Generation.
- Fast, short pauses.
- Objects collected from Eden and moved to Survivor/Old Gen.

### 🔸 Major (Full) GC
- Involves Old Generation and sometimes Metaspace.
- Causes longer application pauses.
- Can severely affect performance if not tuned.

---

## 📝 Summary of JVM Memory Areas

| Memory Area     | Description                                                |
|------------------|------------------------------------------------------------|
| Heap             | Stores objects (Young Gen + Old Gen)                       |
| Stack            | Stores method calls and local variables (thread-local)     |
| Metaspace        | Stores class metadata and JVM structure                    |
| Program Counter  | Tracks current instruction for each thread                 |
| Young Gen        | Short-lived objects (Eden + Survivor)                      |
| Old Gen          | Long-lived objects (promoted from Young Gen)              |

---

💡 **Tip**: Use tools like `VisualVM`, `JConsole`, or `JFR` to visualize memory usage and tune GC parameters with flags like `-Xmx`, `-Xms`, `-XX:+UseG1GC`.

