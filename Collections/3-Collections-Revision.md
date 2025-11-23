#  **Java Collections — Revision Notes**

## **1. What is Java Collections Framework (JCF)?**

* A set of **interfaces + classes** to store and manipulate groups of objects.
* Located in **java.util** package.
* Provides ready-made data structures (List, Set, Map, Queue…).

---

# 🔶 **2. Key Collection Interfaces**

### **A. List (Ordered, allows duplicates)**

* Maintains insertion order.
* Access elements by index.

**Implementations:**

* **ArrayList** → fast access (indexing), slow insert/delete in middle.
* **LinkedList** → fast insert/delete, slow lookup.
* **Vector** → legacy, synchronized.
* **Stack** → legacy stack (better use Deque).

---

### **B. Set (No duplicates)**

* Does not allow duplicate elements.

**Implementations:**

* **HashSet** → fastest, no order.
* **LinkedHashSet** → maintains insertion order.
* **TreeSet** → sorted (ascending order), slower (uses TreeMap internally).

---

### **C. Queue / Deque**

Used for FIFO / LIFO operations.

**Implementations:**

* **PriorityQueue** → ordered by natural order or comparator.
* **ArrayDeque** → best for stack/queue operations (faster than Stack/LinkedList).

---

### **D. Map (Key–Value pairs)**

Keys are unique; values may repeat.

**Implementations:**

* **HashMap** → fastest, no order.
* **LinkedHashMap** → maintains insertion order.
* **TreeMap** → sorted by key.
* **Hashtable** → legacy, synchronized.

---

# 🔶 **3. Important Classes**

### **Collections Class**

* Utility class for operations on collections.
* Common methods:

  * `sort(List)`
  * `min()`, `max()`
  * `reverse()`, `shuffle()`
  * `synchronizedList()`, etc.

### **Arrays Class**

* Utility class for array operations.

  * `sort()`
  * `binarySearch()`
  * `toString()`

---

# 🔶 **4. Iteration Methods**

### **Iterator**

* Works for all collections.
* Methods: `hasNext()`, `next()`, `remove()`

### **ListIterator**

* Only for lists (bidirectional).
* Methods: `hasPrevious()`, `previous()`

### **Enhanced For Loop (for-each)**

---

# 🔶 **5. Differences to Remember**

### **ArrayList vs LinkedList**

| Feature              | ArrayList | LinkedList |
| -------------------- | --------- | ---------- |
| Access               | Fast      | Slow       |
| Insert/Delete middle | Slow      | Fast       |

---

### **HashSet vs TreeSet**

| Feature       | HashSet | TreeSet        |
| ------------- | ------- | -------------- |
| Order         | No      | Sorted         |
| Speed         | Fast    | Slower         |
| Null allowed? | Yes     | No (generally) |

---

### **HashMap vs LinkedHashMap vs TreeMap**

| Map Type      | Order           | Speed           |
| ------------- | --------------- | --------------- |
| HashMap       | No order        | Fastest         |
| LinkedHashMap | Insertion order | Slightly slower |
| TreeMap       | Sorted order    | Slowest         |

---

# 🔶 **6. Generics**

* Used to ensure type safety.
  Example:

```java
List<String> list = new ArrayList<>();
```

Prevents adding other types.

---

# 🔶 **7. Fail-Fast vs Fail-Safe Iterators**

* **Fail-fast** (most of Collections) → throws `ConcurrentModificationException` if collection changes during iteration.
* **Fail-safe** (ConcurrentHashMap, CopyOnWriteArrayList) → no exception, works on copied structure.
---
# 🔶 **8. Common Interview Points**

* List allows duplicates; Set does not.
* Map stores key–value pairs.
* HashMap allows one null key; Hashtable does not.
* `ConcurrentHashMap` replaces `Hashtable` in modern code.
* Prefer **ArrayDeque** over Stack.

---
<details>
  <summary><h1>Few Interview Questions</h1></summary>
  
## **1. Difference between List, Set, and Map?**

* **List** → Ordered, allows duplicates.
* **Set** → Unordered (or sorted), no duplicates.
* **Map** → Key–value pairs, keys are unique.

---

## **2. Why is HashMap fast?**

Because it uses:

* **Hashing** (O(1) average time)
* **Array + LinkedList/Tree (after Java 8)**
  If too many collisions → bucket becomes a Red-Black Tree.

---

## **3. What changed in HashMap in Java 8?**

* Buckets convert to **Red-Black Trees** when size > 8.
* Improves worst-case performance from **O(n)** to **O(log n)**.

---

## **4. Difference between HashMap and Hashtable?**

| HashMap             | Hashtable    |
| ------------------- | ------------ |
| Not synchronized    | Synchronized |
| Faster              | Slower       |
| Allows one null key | No null      |
| Modern code         | Legacy       |

---

## **5. Difference between HashMap and ConcurrentHashMap?**

* **ConcurrentHashMap** uses segment locking / CAS for concurrency.
* Safe for multi-threading.
* No `null` keys/values.
* Iterators are **fail-safe**.

---

## **6. Why is HashSet slower for iteration than ArrayList?**

Because HashSet has **no index**, elements are stored in **hash buckets**, not continuous memory.

---

## **7. How does HashSet work internally?**

* Uses **HashMap** internally.
* Values stored in map are a constant dummy object.

---

## **8. What is the difference between TreeSet and HashSet?**

* TreeSet → **sorted**, uses **TreeMap (Red-Black Tree)**.
* HashSet → **unsorted, faster**.

---

## **9. What is the load factor in HashMap?**

* Load factor = when to **resize** HashMap.
* Default = **0.75** → Resize when 75% full.
* Resizing is expensive (rehashing).

---

## **10. What is the initial capacity of HashMap?**

* Default capacity = **16**
* After resizing → capacity doubles (16 → 32 → 64 …)

---

## **11. What is the difference between Iterator and ListIterator?**

* Iterator → Single direction.
* ListIterator → Both directions, only for List.

---

## **12. What is Fail-Fast?**

If you modify a collection while iterating, most iterators throw:
👉 **ConcurrentModificationException**

---

## **13. What is Fail-Safe?**

Works on a **copy** of the collection.
No exception if modified concurrently.
Examples:

* ConcurrentHashMap
* CopyOnWriteArrayList

---

## **14. Why doesn’t ConcurrentHashMap allow null keys?**

Because:

* Null keys make it impossible to distinguish between
  **“key absent”** and **“key present with null value”** in concurrent environment.

---

## **15. How to make a Collection thread-safe?**

* Use synchronized wrappers:

  ```java
  List l = Collections.synchronizedList(new ArrayList<>());
  ```
* Or use concurrent collections:

  * ConcurrentHashMap
  * CopyOnWriteArrayList
  * ConcurrentLinkedQueue

---

## **16. What is the difference between Comparable and Comparator?**

* **Comparable** → natural ordering (`compareTo`), modifies class.
* **Comparator** → external ordering (`compare`), used outside class.

---

## **17. How does PriorityQueue work internally?**

* Uses **Binary Heap** (min-heap by default).
* Not sorted when viewed, but root is smallest element.

---

## **18. Why is ArrayDeque recommended instead of Stack?**

* Faster
* No synchronization overhead
* Cleaner API
* Modern replacement for Stack

---

## **19. Can you sort a HashMap?**

Not directly.
To sort:

* By key → use TreeMap
* By keys/values → use Streams or List of Map.Entry

---

## **20. What is the difference between poll() and remove() in Queue?**

* `poll()` → returns null if empty
* `remove()` → throws exception if empty

---

## **21. How do you iterate safely on a collection while modifying it?**

* Using **Iterator.remove()**
* Using **CopyOnWriteArrayList**
* Using **ConcurrentHashMap**

---

## **22. Why are Collections not thread-safe by default?**

To avoid:

* Performance overhead (locking)
* Unnecessary synchronization for single-thread use
</details>

---

<details>
  <summary> <h1>Comparable vs Comparator</h1> </summary>

## **1️⃣ Comparable (Natural Ordering)**

* Sorting logic is **inside the class itself**.
* The class must **implement Comparable<T>**.
* Only **one** natural ordering is possible.
* Method to implement:

```java
public int compareTo(T other)
```

### Simple example:

```java
class Student implements Comparable<Student> {
    int marks;

    Student(int m) { this.marks = m; }

    public int compareTo(Student other) {
        return this.marks - other.marks;   // natural order = by marks
    }
}
```

Now any list of `Student` objects will automatically sort by **marks**.

---

## **2️⃣ Comparator (External Ordering)**

* Sorting logic is **outside the class**.
* Can create **multiple comparators** for different sorting rules.
* Method to implement:

```java
public int compare(T a, T b)
```

### Simple example:

```java
class SortByMarks implements Comparator<Student> {
    public int compare(Student a, Student b) {
        return a.marks - b.marks;   // external order = by marks
    }
}
```

Use it like:

```java
Collections.sort(studentList, new SortByMarks());
```

---

# ⭐ **TABLE — Comparable vs Comparator Methods**

Including **null-safe** vs **throws exception**

| Feature                        | Comparable                                                       | Comparator                                                                      |
| ------------------------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Interface from                 | java.lang                                                        | java.util                                                                       |
| Main method                    | `compareTo(T o)`                                                 | `compare(T o1, T o2)`                                                           |
| Natural/External ordering      | Natural                                                          | External (custom)                                                               |
| Sort using                     | `Collections.sort(list)`                                         | `Collections.sort(list, comparator)`                                            |
| Number of orderings possible   | Only one                                                         | Many                                                                            |
| Presence of null allowed?      | ❌ **Throws NullPointerException** when calling `compareTo(null)` | ✔️ **Null-safe methods exist** like `Comparator.nullsFirst()` and `nullsLast()` |
| Can sort null values directly? | No                                                               | Yes (if using null-handling comparator)                                         |

---

# ⭐ **Comparator Special Helper Methods (SAFE for NULL)**

Java provides built-in **null-handled comparators**:

### ✔ Null-safe versions (do NOT throw exception)

| Method                       | Meaning                     |
| ---------------------------- | --------------------------- |
| `Comparator.nullsFirst(cmp)` | Nulls come before non-nulls |
| `Comparator.nullsLast(cmp)`  | Nulls come after non-nulls  |

Example:

```java
Comparator<String> cmp = Comparator.nullsFirst(String::compareTo);
```

This will **not throw** `NullPointerException` even if list contains null.

---

# ❌ **Methods that throw NullPointerException**

| Interface  | Method            | Throws null exception when…                                                         |
| ---------- | ----------------- | ----------------------------------------------------------------------------------- |
| Comparable | `compareTo(null)` | Always throws NPE                                                                   |
| Comparator | `compare(o1, o2)` | If your compare logic uses methods on null objects (e.g., `o1.name.compareTo(...)`) |

Example that throws:

```java
Comparator<String> cmp = (a, b) -> a.compareTo(b); // throws NPE if a or b is null
```

---

# ✔ **Simple Visual Summary**

| Aspect             | Comparable                       | Comparator                      |
| ------------------ | -------------------------------- | ------------------------------- |
| Method             | compareTo()                      | compare()                       |
| Location           | Inside class                     | Outside class                   |
| Custom sorting     | ❌ No (only one)                  | ✔ Yes (many)                    |
| Null-safe versions | ❌ No                             | ✔ nullsFirst(), nullsLast()     |
| Best when          | Class has one natural sort order | You want different ways to sort |

---


# ✅ **Expression:**

```java
(a, b) -> a.compareTo(b)
```

# ⭐ **This is a Comparator.**

Even though it **uses `compareTo()` internally**, the **lambda structure defines a Comparator** because:

* A **Comparator** takes **two arguments** → `(a, b)`
* A **Comparable** takes **one argument** → `compareTo(T o)`

So the moment you see **two parameters**, it belongs to **Comparator**, not Comparable.

---

# 🧠 Short explanation:

### ✔ Comparator interface:

```java
int compare(T a, T b);
```

### ✔ Lambda version:

```java
(a, b) -> a.compareTo(b)
```

You are implementing the `compare(a, b)` method.
Therefore → **Comparator**.

---

# 🚫 What Comparable looks like:

```java
obj.compareTo(otherObj);
```

OR lambda-like idea:

```
(a) -> a.compareTo(...)
```

(But Comparable cannot be created via lambda directly because it’s not a functional interface.)

---

# ✔ Why does Comparator call compareTo?

Because we often reuse natural ordering like this:

```java
Comparator<String> cmp = (a, b) -> a.compareTo(b);
```

This produces the same ordering as natural ordering, but still acts as a **Comparator**.

---

**`(a, b) -> a.compareTo(b)` = Comparator (not Comparable)**.


</details>

---

<details>
  <summary><h1> Java Collections — Methods That Return NULL vs Throw EXCEPTION</h1></summary>
  
## **1. Queue / Deque Methods**

| Operation                          | Safe Method (returns **null**)           | Unsafe Method (**throws exception**)                               |
| ---------------------------------- | ---------------------------------------- | ------------------------------------------------------------------ |
| **Retrieve head without removing** | `peek()` → returns null if empty         | `element()` → throws `NoSuchElementException`                      |
| **Retrieve + remove head**         | `poll()` → returns null if empty         | `remove()` → throws `NoSuchElementException`                       |
| **Add to queue**                   | `offer(e)` → returns false if cannot add | `add(e)` → throws `IllegalStateException` if full (bounded queues) |

---

# ⭐ Example

```java
Queue<Integer> q = new LinkedList<>();

q.peek();   // null
q.element(); // throws NoSuchElementException

q.poll();   // null
q.remove(); // throws NoSuchElementException
```

---

## **2. Stack / Deque (LIFO operations)**

(*Using Deque — recommended over legacy Stack*)

| Operation | Safe Method (null on empty) | Unsafe Method (throws exception) |
| --------- | --------------------------- | -------------------------------- |
| **Pop**   | `pollLast()`                | `removeLast()` or `pop()`        |
| **Peek**  | `peekLast()`                | `getLast()`                      |

---

## **3. Map Methods**

| Access Type         | Null-safe (returns null)                           | Throws exception                                                     |
| ------------------- | -------------------------------------------------- | -------------------------------------------------------------------- |
| **Get value**       | `get(key)` → returns null if key absent            | ❌ none (Maps don’t throw for missing key)                            |
| **Remove key**      | `remove(key)` → returns null if key wasn’t present | ❌ none                                                               |
| **Compute methods** | `getOrDefault(key, default)`                       | `compute()` throws NPE if function returns null for non-nullable map |

---

## **4. List Methods**

| Operation           | Safe Method | Unsafe Method                                        |
| ------------------- | ----------- | ---------------------------------------------------- |
| **Access by index** | ❌ none      | `get(index)` → throws `IndexOutOfBoundsException`    |
| **Remove by index** | ❌ none      | `remove(index)` → throws `IndexOutOfBoundsException` |

Lists generally **do not return null for invalid index** — they throw exceptions.

---

## **5. Set Methods**

| Operation          | Safe Method                              | Unsafe Method |
| ------------------ | ---------------------------------------- | ------------- |
| **Check presence** | `contains(obj)` → returns false          | ❌ none        |
| **Remove**         | `remove(obj)` → returns false if missing | ❌ none        |

Sets **never throw exceptions for missing elements**.

---

## **6. BlockingQueue (special concurrency)**

| Safe Method (timeout / null) | Throws Exception    |
| ---------------------------- | ------------------- |
| `offer(e, timeout)`          | `add(e)` if full    |
| `poll(timeout)`              | `remove()` if empty |
| `put(e)` → waits             | ❌ none              |
| `take()` → waits             | ❌ none              |

---

# 🎯 **Complete Summary Table**

| Collection Type   | Safe Methods (return null / false)         | Unsafe Methods (throw exception)                     |
| ----------------- | ------------------------------------------ | ---------------------------------------------------- |
| **Queue**         | `peek()` / `poll()` / `offer()`            | `element()` / `remove()` / `add()`                   |
| **Deque**         | `peekFirst()` / `peekLast()` / `poll...()` | `getFirst()` / `getLast()` / `remove...()` / `pop()` |
| **List**          | ❌ none for index errors                    | `get(index)` / `remove(index)`                       |
| **Set**           | `contains()` / `remove(obj)`               | ❌ none                                               |
| **Map**           | `get()` / `remove()` / `getOrDefault()`    | Rare cases → compute failures                        |
| **BlockingQueue** | `offer(timeout)` / `poll(timeout)`         | `add()` / `remove()`                                 |

---
</details>
