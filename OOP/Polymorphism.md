### 📘 Definition

**Polymorphism** means *“many forms”* — it allows a single interface (method or reference) to represent different underlying forms (implementations).

In simple terms:

> The same method call or reference can behave differently depending on **context** — i.e., which method is chosen or which object type is being referred to.

---

### 🧩 Types of Polymorphism in Java

| Type             | Also Called          | Binding Time       | Mechanism              |
| ---------------- | -------------------- | ------------------ | ---------------------- |
| **Compile-time** | Static Polymorphism  | During compilation | **Method Overloading** |
| **Runtime**      | Dynamic Polymorphism | During execution   | **Method Overriding**  |

---

## ⚙️ 1. Compile-time (Static) Polymorphism — *Method Overloading*

### 📖 Concept

In *method overloading*, multiple methods in the same class share the **same name** but differ in **parameter list** (type, number, or order).
The **compiler** decides which version to call, based on the arguments at compile time.

---

### 🧩 Example: Method Overloading

```java
class Calculator {
    // Different parameter types
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }

    // Different parameter count
    int add(int a, int b, int c) { return a + b + c; }

    // Different parameter order
    void printInfo(String name, int age) {
        System.out.println(name + " is " + age + " years old.");
    }

    void printInfo(int age, String name) {
        System.out.println(name + " is " + age + " years old.");
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator c = new Calculator();
        System.out.println(c.add(2, 3));        // calls add(int, int)
        System.out.println(c.add(2.5, 3.7));    // calls add(double, double)
        c.printInfo("Alice", 22);
    }
}
```

✅ The compiler resolves which `add()` or `printInfo()` to call based on the **argument types**.

---

### 🚫 What Does *Not* Count as Overloading

* Changing only the **return type** is **not** overloading.

  ```java
  int add(int a, int b) { return a + b; }
  double add(int a, int b) { return a + b; } // ❌ compile error
  ```

* Changing only **access modifiers** or **exception lists** also doesn’t overload.

---

### 💡 Why It Matters

Overloading improves **readability**, **code reuse**, and **method flexibility** — allowing intuitive names without clutter.

---

## ⚙️ 2. Runtime (Dynamic) Polymorphism — *Method Overriding*

### 📖 Concept

In *method overriding*, a subclass redefines a **method with the same signature** as its parent class.
The actual method to be called is determined **at runtime**, based on the **object** that the reference points to.

---

### 🧩 Example: Method Overriding

```java
class Animal {
    void makeSound() {
        System.out.println("Some generic animal sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof woof");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog(); // upcasting
        a.makeSound(); // Output: Woof woof
    }
}
```

✅ Even though `a` is of type `Animal`, the **actual object** is `Dog`,
so the JVM calls `Dog`’s `makeSound()` — this is **runtime dispatch**.

---

### 💡 Important Notes

| Concept              | Description                                                |
| -------------------- | ---------------------------------------------------------- |
| **Method Signature** | Must match exactly (name + parameter list).                |
| **@Override**        | Recommended to ensure correctness.                         |
| **Access Modifier**  | Cannot be more restrictive than in the parent.             |
| **Exceptions**       | Overriding method cannot throw broader checked exceptions. |
| **Return Type**      | Can be *covariant* (return subtype).                       |

---

### 🧩 Upcasting and Downcasting

```java
Animal a = new Dog(); // ✅ Upcasting (safe)
Dog d = (Dog) a;      // ✅ Downcasting (only safe if object is actually Dog)
```

* **Upcasting**: assigning a subclass object to a superclass reference.

  * Allows *generalization* (treat different subclasses the same).
* **Downcasting**: assigning a superclass reference back to a subclass.

  * Must be done with care; can cause `ClassCastException`.

---

### ⚙️ Why Dynamic Polymorphism Matters

* Enables **extensibility** (Open–Closed Principle): new subclasses can override behaviors without touching parent code.
* Supports **loose coupling** and **interface-driven design**.
* Essential in **frameworks** (Spring, Hibernate, etc.) that depend on dynamic binding.

---

### 🧠 Tricky Interview Snippets

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

✅ **Method call** decided at runtime,
❌ **Available methods** decided at compile time (based on reference type).

---

#### 🔹 2. Field Hiding vs Method Overriding

```java
class Parent {
    String name = "Parent";
}

class Child extends Parent {
    String name = "Child";
}

Parent p = new Child();
System.out.println(p.name); // Parent
```

⚠️ Fields are *not polymorphic*!
Only **methods** participate in polymorphism.

---

#### 🔹 3. Static Methods Are *Not* Polymorphic

```java
class Base {
    static void display() { System.out.println("Base static"); }
}

class Derived extends Base {
    static void display() { System.out.println("Derived static"); }
}

Base b = new Derived();
b.display(); // Base static
```

⚠️ Static methods are **resolved at compile time** — this is *method hiding*, not overriding.

---

### 💬 Common Interview Questions

1. Can constructors be overridden? → ❌ No, only inherited methods can.
2. Can we override `private` or `static` methods? → ❌ No.
3. Why is overriding called *runtime* polymorphism? → Because the call is resolved based on the actual object at runtime.
4. What happens if we remove `@Override` annotation? → It still works, but may silently fail if method signature mismatches.
5. Which principle does it support? → **Open–Closed Principle** (OCP).

---

### ⚙️ Best Practices

* Always use `@Override` to avoid signature mismatches.
* Prefer **upcasting** to generalize code.
* Avoid **downcasting** unless absolutely necessary.
* Use **interfaces or abstract classes** to design extensible polymorphic systems.

---

### 🧭 Quick Summary Table

| Type         | When Decided    | Achieved By        | Example                          | Key Rule                      |
| ------------ | --------------- | ------------------ | -------------------------------- | ----------------------------- |
| Compile-time | At compile time | Method Overloading | Same method name, diff params    | Return type alone not enough  |
| Runtime      | At runtime      | Method Overriding  | Subclass redefines parent method | Based on object, not ref type |

---
