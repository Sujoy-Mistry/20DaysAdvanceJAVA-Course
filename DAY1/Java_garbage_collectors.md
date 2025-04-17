
# 🗑️ Types of Garbage Collectors in Java

## 1. Serial Garbage Collector (Serial GC)
**Command to enable:** `-XX:+UseSerialGC`

### 📌 Description:
- A **single-threaded collector** for both minor and major garbage collection.
- It pauses all application threads (stop-the-world) during garbage collection.
- Best suited for **small applications** or **environments with limited CPU resources**.

### 🔧 How it works:
- Uses a **copying collector** for the Young Generation (from Eden to Survivor spaces).
- Uses a **mark-sweep-compact** algorithm for the Old Generation.

### ✅ Pros:
- Simple and low-overhead.
- Works well for single-threaded or small heap applications.

### ❌ Cons:
- Causes long **stop-the-world pauses** during GC.
- Not suitable for applications requiring low latency or running on multi-core CPUs.

### 🔍 Use Case:
- Embedded systems.
- Desktop applications with small memory footprints.

---

## 2. Parallel Garbage Collector (Throughput Collector)
**Command to enable:** `-XX:+UseParallelGC` (default before Java 9)

### 📌 Description:
- Uses **multiple threads** to perform minor garbage collections, which increases **throughput**.

### 🔧 How it works:
- Parallelizes the work done in the Young Generation.
- For the Old Generation, it uses a **parallel mark-sweep-compact** algorithm.

### ✅ Pros:
- Good for applications that prioritize **throughput over latency**.
- Makes use of available CPU cores efficiently.

### ❌ Cons:
- Still involves **stop-the-world** pauses.
- Not ideal for low-latency or real-time systems.

### 🔍 Use Case:
- Batch processing.
- Server-side applications where **pause time is acceptable**.

---

## 3. Concurrent Mark Sweep (CMS) Garbage Collector
**Command to enable:** `-XX:+UseConcMarkSweepGC`

### 📌 Description:
- A low-pause GC that tries to **minimize application pauses** by doing most of the GC work concurrently.

### 🔧 How it works:
- Divides GC into **phases**, some of which are concurrent.
- Old Generation is collected using a **mark-sweep** approach (no compaction unless explicitly requested).

### ✅ Pros:
- **Shorter GC pauses**, better responsiveness.
- Well-suited for apps that require **low latency**.

### ❌ Cons:
- Uses **more CPU**.
- Fragmentation in Old Generation.
- Deprecated as of Java 9, removed in Java 14.

### 🔍 Use Case:
- Web servers or user-facing applications requiring **quick response times**.

---

## 4. G1 Garbage Collector (Garbage-First GC)
**Command to enable:** `-XX:+UseG1GC` (default from Java 9+)

### 📌 Description:
- Designed for **predictable pause times** and **efficient handling of large heaps**.
- Splits the heap into **regions**, not fixed Young/Old generations.

### 🔧 How it works:
- Performs garbage collection on a **region basis**.
- Young and Old GC phases can be concurrent.
- Uses a **predictive model** to keep pause times below a target threshold (`-XX:MaxGCPauseMillis`).

### ✅ Pros:
- **Pause-time predictability**.
- Works well with large heaps.
- Concurrent compaction reduces fragmentation.

### ❌ Cons:
- More complex to tune.
- May have **higher CPU usage** than simpler collectors.

### 🔍 Use Case:
- **Large-scale** enterprise apps.
- Services where **low latency and large heap** are both important.

---

## 🧠 Summary Table

| GC Type         | Pause Time     | Concurrency | Heap Size Suitability | Use Case                          |
|------------------|----------------|-------------|------------------------|-----------------------------------|
| **Serial GC**    | Long           | No          | Small heaps            | Small desktop or embedded apps   |
| **Parallel GC**  | Medium         | No (multi-threaded collection) | Medium to large     | Batch processing, throughput-first |
| **CMS GC**       | Low            | Yes         | Medium to large        | Web servers, latency-sensitive apps |
| **G1 GC**        | Predictable    | Yes         | Large heaps            | Enterprise-scale apps             |
