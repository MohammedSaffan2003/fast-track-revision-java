### 📘 Definition

**Encapsulation** is the process of **wrapping data (fields)** and **methods (behavior)** that operate on that data into a **single unit**, called a **class**.

It also involves **restricting direct access** to the class’s fields and providing **controlled access** through public methods.

> In simple terms: *Hide the data and expose behavior.*

---

### 💡 Why Encapsulation?

* To **protect sensitive data** from unintended modification.
* To **control how data is accessed or changed**.
* To **achieve maintainability** — internal changes don’t break external code.
* To **achieve abstraction** — hides implementation details.

---

### 🧱 Example: Encapsulation in Action

```java
class Account {
    private double balance; // hidden from outside access

    // Constructor
    public Account(double initialBalance) {
        if (initialBalance >= 0)
            balance = initialBalance;
    }

    // Getter (read-only access)
    public double getBalance() {
        return balance;
    }

    // Setter (controlled write access)
    public void deposit(double amount) {
        if (amount > 0)
            balance += amount;
    }
}

public class Main {
    public static void main(String[] args) {
        Account acc = new Account(1000);
        acc.deposit(500);
        System.out.println("Balance: " + acc.getBalance());

        // acc.balance = -100; // ❌ Not allowed — private field
    }
}
```

**Output:**

```
Balance: 1500.0
```

✅ The `balance` variable is hidden and can only be modified via safe methods.

---

### 🧩 Access Modifiers in Java

| Modifier                    | Class Visibility      | Package Visibility | Subclass (Other Package) | World (Everywhere) |
| --------------------------- | --------------------- | ------------------ | ------------------------ | ------------------ |
| **private**                 | ✅ (only inside class) | ❌                  | ❌                        | ❌                  |
| **default** *(no modifier)* | ✅                     | ✅                  | ❌                        | ❌                  |
| **protected**               | ✅                     | ✅                  | ✅                        | ❌                  |
| **public**                  | ✅                     | ✅                  | ✅                        | ✅                  |

> 💬 Tip: For **maximum encapsulation**, mark fields as `private` and expose them via `public` getters/setters only when necessary.

---

### 🔒 Data Hiding

Encapsulation enables **data hiding**, which means:

* Fields cannot be accessed directly from outside the class.
* Only public methods (getters/setters) decide how to access or modify data.

```java
class Student {
    private int age;

    public void setAge(int age) {
        if (age > 0 && age < 100)
            this.age = age;
        else
            System.out.println("Invalid age");
    }

    public int getAge() {
        return age;
    }
}
```

---

### 🧠 Encapsulation vs Abstraction

| Concept           | Focus                                                                 | Example                              |
| ----------------- | --------------------------------------------------------------------- | ------------------------------------ |
| **Encapsulation** | Hides **data** and restricts access.                                  | Private fields with getters/setters. |
| **Abstraction**   | Hides **implementation details** and exposes only essential features. | Interfaces, abstract classes.        |

🔸 Encapsulation is about *how data is protected*.
🔸 Abstraction is about *what behavior is exposed*.

---

### 🧩 Encapsulation in Real Classes

Java’s built-in classes use encapsulation extensively.

Example:

```java
String s = "Hello";
```

`String` class is **immutable** — once created, its internal value can’t be changed.
That’s achieved through **encapsulation + immutability** (fields are private, no setters).

---

### 🧠 Tricky Interview Snippets

#### 🔹 1. Changing Access Levels

```java
class A {
    protected int value;
}
class B extends A {
    private int value; // ❌ Hides parent's field, doesn’t override it
}
```

✅ You’re not overriding the variable; you’re **declaring a new one** (shadowing).

---

#### 🔹 2. Getters and Setters Bypass

```java
class Test {
    private int x;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        x = x; // ❌ Wrong! It sets parameter to itself
    }
}
```

✅ Fix: Use `this.x = x;` to refer to instance variable.

---

#### 🔹 3. Partial Encapsulation

```java
class Person {
    public String name; // ❌ breaks encapsulation
    private int age;
}
```

✅ Even though it’s syntactically valid, it’s a **bad design** — breaks control and allows invalid state.

---

#### 🔹 4. Controlled Setters

```java
class Account {
    private double balance;

    public void setBalance(double balance) {
        if (balance >= 0)
            this.balance = balance;
        else
            System.out.println("Negative balance not allowed");
    }
}
```

✅ Encapsulation lets you **validate and sanitize data** before changing the internal state.

---

### 💡 Best Practices

1. Keep fields **private** by default.
2. Provide **getters/setters** only if external access is truly needed.
3. Add **validation logic** in setters to prevent invalid states.
4. Combine encapsulation with **immutability** for safer designs.
5. Never expose **mutable objects** directly — return **copies** if needed.

---

### 🧭 Quick Summary Table

| Feature          | Encapsulation Benefit                    |
| ---------------- | ---------------------------------------- |
| Private fields   | Protects data from external modification |
| Getters/Setters  | Provide controlled access                |
| Access Modifiers | Define visibility boundaries             |
| Data Hiding      | Prevents direct manipulation             |
| Immutability     | Ensures stable and safe objects          |

---
