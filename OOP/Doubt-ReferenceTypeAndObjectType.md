#### 🔹 1. Reference vs Object Type

```java
class A {
    void show() { System.out.println("A's show"); }
}

class B extends A {
    void show() { System.out.println("B's show"); }
    void onlyInB() { System.out.println("Only in B"); }
}

public class Test {
    public static void main(String[] args) {
        A obj = new B();
        obj.show();      // B's show
        // obj.onlyInB(); // ❌ compile error (ref type = A)
    }
}
```
 ## Solution

### 🔹 1. The Core Concept

* In Java, every object is **accessed through a reference variable**.
* A **reference type** acts like a *label* or *container* that points to an actual **object** in memory.

```java
A obj = new B();
```

Here:

* `A` → **reference type**
* `new B()` → **object type (runtime type)**

---

### 🔹 2. What Each One Controls

| Aspect                                   | Controlled by    | Explanation                               |
| ---------------------------------------- | ---------------- | ----------------------------------------- |
| **What methods can be called**           | ✅ Reference Type | Checked at **compile time**               |
| **Which version of the method executes** | ✅ Object Type    | Decided at **runtime** (dynamic dispatch) |

---

### 🔹 3. Key Rule

> At **compile time**, Java only looks at the **reference type** to decide what’s legal to call.
> At **runtime**, Java uses the **object type** to decide which overridden method actually runs.

---

### 🔹 4. Example

```java
A obj = new B();
obj.show();     // ✅ Allowed — method exists in A; runtime runs B’s version
obj.onlyInB();  // ❌ Compile-time error — not defined in A
```

**Reason:**

* The compiler checks: "Does class `A` have a `onlyInB()` method?" → No.
* So it fails **before** the program even runs.

---

### 🔹 5. Why Java Does This

* The compiler must guarantee **type safety** — it can’t assume what the runtime object will be.
* If the compiler allowed subclass-only methods on a parent reference, this could happen:

```java
A obj = new A(); 
obj.onlyInB(); // 💥 would crash at runtime if obj is not actually a B
```

So Java forbids it at compile time.

---

### 🔹 6. How to Access Subclass-Specific Methods (Safely)

If you *know* the object is really of type `B`, you can use **downcasting**:

```java
((B) obj).onlyInB();  // ✅ Works if obj really points to a B object
```

Always check before casting:

```java
if (obj instanceof B) {
    ((B) obj).onlyInB();
}
```

---

### 🔹 7. The “Container” Analogy Refined

* The **reference** (`A`) is like a **box label** — it says what operations you’re allowed to perform.
* The **object** (`B`) is the **actual thing inside the box**.
* Even if the object can do more, the compiler only trusts the label on the box.

If you want to use the object’s extra features, you must **relabel the box** (cast it).

---

### 🔹 8. Why This Is Useful

* Enables **polymorphism** — you can treat many subclass objects uniformly via a parent reference.
* Allows writing flexible and reusable code:

  ```java
  List<Animal> zoo = new ArrayList<>();
  zoo.add(new Dog());
  zoo.add(new Cat());

  for (Animal a : zoo) {
      a.makeSound(); // Polymorphic behavior
  }
  ```

---

### 🔹 9. Summary Table

| Concept                 | Controlled by                    | Checked When      | Example                        |
| ----------------------- | -------------------------------- | ----------------- | ------------------------------ |
| Method visibility       | Reference type                   | Compile time      | `A obj = new B(); obj.show();` |
| Actual method execution | Object type                      | Runtime           | Executes B’s `show()`          |
| New methods in subclass | Not visible via parent reference | Compile time      | Causes error                   |
| Downcasting             | Changes reference type           | Compile + Runtime | `((B)obj).onlyInB();`          |

---
### 🔹 10. One-line Summary

> **The reference type decides *what you can do*;
> the object type decides *how it’s done*.**

---

<details>
    <summary>Example considering the List and ArrayList</summary>
    
### 1️⃣ Reference type vs object type

In Java:

* **Reference type** → what the compiler sees, what methods you’re allowed to call.
* **Object type** → the actual runtime object, which may have more methods.

```java
List<Integer> list = new ArrayList<>();
list.add(10);         // ✅ allowed because add() is in List
// list.ensureCapacity(100);  // ❌ compiler error! Not in List
```

* Even though `list` **is actually an ArrayList**, the compiler only allows **methods in List**.
* If you want `ArrayList`-specific methods:

```java
((ArrayList<Integer>) list).ensureCapacity(100); // ✅ works
```

---

### 2️⃣ Why use a broader type?

Using a broader type like `List` is actually **a design choice**, not a limitation:

1. **Flexibility**: Your code can accept *any* List implementation (ArrayList, LinkedList, CopyOnWriteArrayList…).

   ```java
   void processList(List<String> l) { ... }
   ```

   You don’t care if it’s ArrayList or LinkedList — just that it behaves like a List.

2. **Decoupling**: Your code depends on the interface, not the specific implementation.

   * Makes swapping implementations easier later.
   * Makes testing easier (you could pass a mock List, for example).

3. **Safety**: Forces you to write code that uses **general behavior**, rather than relying on class-specific tricks.

---

### 3️⃣ Trade-off

* ✅ **Pros**: Flexibility, easier maintenance, polymorphism.
* ⚠️ **Cons**: You lose access to class-specific methods unless you cast.

Think of it like **using a universal remote**. You can control any TV (interface methods), but you can’t use brand-specific features unless you switch to the TV’s actual remote (casting).

---
</details>
