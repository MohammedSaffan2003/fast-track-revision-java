### 📘 Definition

**Immutability** means **an object’s state cannot change after it’s created.**

Once you assign values to an object’s fields, you can’t modify them — instead, any change produces a **new object**.

This is a **stronger form of encapsulation** — not only is the data hidden, but it’s also **locked** against modification.

---

### 💡 Relationship Between Encapsulation and Immutability

| Concept           | What It Does                                                                     | How They Complement             |
| ----------------- | -------------------------------------------------------------------------------- | ------------------------------- |
| **Encapsulation** | Hides and protects data using access control (private fields + getters/setters). | Prevents *uncontrolled* access. |
| **Immutability**  | Prevents modification after object creation.                                     | Prevents *any* state change.    |

✅ Together they create **safe, thread-proof, and predictable objects**.
Encapsulation restricts “who can touch the data,”
Immutability ensures “nobody can change the data.”

---

### 🧱 How to Create an Immutable Class in Java

To make a class immutable:

1. Declare the class as `final` (cannot be subclassed).
2. Make all fields `private` and `final`.
3. Don’t provide any setters.
4. Initialize fields only in the constructor.
5. Don’t expose mutable objects directly (return copies instead).

---

### 🧩 Example: Custom Immutable Class

```java
final class Student {
    private final String name;
    private final int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // only getters, no setters
    public String getName() { return name; }
    public int getAge() { return age; }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Alice", 22);
        System.out.println(s1.getName());

        // s1.age = 25; // ❌ not allowed
        // s1.setName("Bob"); // ❌ no setter provided
    }
}
```

✅ The internal state of `Student` never changes after creation.

---

### 🧩 Example: Immutability with Mutable Fields

When a class has a field that refers to a **mutable object**, like `Date` or `List`,
you must **defend** it by returning a **copy**, not the original.

```java
import java.util.Date;

final class Employee {
    private final String name;
    private final Date joiningDate;

    public Employee(String name, Date joiningDate) {
        this.name = name;
        // defensive copy
        this.joiningDate = new Date(joiningDate.getTime());
    }

    public String getName() { return name; }

    public Date getJoiningDate() {
        // return copy instead of actual field
        return new Date(joiningDate.getTime());
    }
}
```

✅ Prevents external modification of the internal `Date` field.

---

### 💡 How Java’s `String` Class Implements Immutability

* The `String` class in Java is **final**, so it can’t be subclassed.
* It has **private final char[] value** field to store data.
* It provides **no setters**, only operations that create **new String objects**.

```java
String s1 = "Hello";
String s2 = s1.concat(" World");

System.out.println(s1); // Hello
System.out.println(s2); // Hello World
```

✅ The original `s1` never changes — `concat()` returns a new object.

---

### 💡 Wrapper Classes are Immutable Too

All wrapper classes like `Integer`, `Double`, `Boolean`, etc., are **immutable**.

```java
Integer x = 10;
Integer y = x;
x = x + 5;
System.out.println(y); // 10 (unchanged)
System.out.println(x); // 15 (new object)
```

✅ `x + 5` creates a **new Integer object**; it doesn’t modify the existing one.

---

### 🧠 Tricky Interview Snippets

#### 🔹 1. Misleading Mutability

```java
final class Person {
    private final StringBuilder address = new StringBuilder("Delhi");
    public StringBuilder getAddress() { return address; }
}

Person p = new Person();
p.getAddress().append(", India");
System.out.println(p.getAddress());
```

**Output:**

```
Delhi, India
```

❌ The class looks immutable but isn’t — because `StringBuilder` (a mutable object) is exposed directly.

✅ Fix: Return a copy in the getter.

```java
public StringBuilder getAddress() {
    return new StringBuilder(address.toString());
}
```

---

#### 🔹 2. Defensive Copy Proof

```java
Date date = new Date();
Employee e = new Employee("Tom", date);

date.setTime(0); // trying to change external Date
System.out.println(e.getJoiningDate()); // unchanged
```

✅ The internal date didn’t change because the class made a **defensive copy**.

---

#### 🔹 3. Thread Safety Advantage

Immutable objects are **naturally thread-safe** — no synchronization needed.

```java
String s = "Java";
Runnable r = () -> System.out.println(s.concat("!"));

new Thread(r).start();
new Thread(r).start();
```

✅ Both threads safely use `s` without conflict — because `String` is immutable.

---

### 🔒 Benefits of Immutability

| Benefit                    | Description                                            |
| -------------------------- | ------------------------------------------------------ |
| **Thread-safety**          | No synchronization needed.                             |
| **Predictability**         | Objects don’t change unexpectedly.                     |
| **Caching & Optimization** | Safe to cache immutable objects (used in String pool). |
| **Simpler debugging**      | Fewer side effects due to no state changes.            |
| **Safe sharing**           | Can be shared between methods/threads without risk.    |

---

### ⚙️ When *Not* to Use Immutability

* When you need **frequent updates** to large objects — creating new ones repeatedly can impact performance.
* In such cases, prefer **mutable classes** like `StringBuilder`, `ArrayList`, etc.

---

### 🧭 Best Practices

1. Use **final fields** and **no setters** for immutability.
2. Make defensive copies of mutable fields in constructors/getters.
3. Avoid exposing internal mutable objects.
4. Combine **encapsulation + immutability** for critical data models (IDs, configs, credentials).
5. Remember: *immutability = strong encapsulation + final state.*

---

### 🧩 Quick Summary Table

| Concept | Keyword/Mechanism                | Purpose                        |
| ------- | -------------------------------- | ------------------------------ |
| Class   | `final`                          | Prevents subclass modification |
| Fields  | `private final`                  | Prevents reassignment          |
| Setters | ❌ Avoided                        | Prevents mutation              |
| Getters | ✅ Return copies                  | Safe access                    |
| Example | `String`, `Integer`, `LocalDate` | Common immutable classes       |

---
