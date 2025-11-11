### 🧠 Definition

**Abstraction** is the process of **hiding the internal implementation details** and **exposing only the essential features** of an object.

Think of it as answering:

> “**What** an object can do” rather than “**How** it does it.”

It focuses on *behavioral design* — making sure classes communicate through well-defined interfaces, not internal logic.

---

### 🎯 Why Abstraction Matters

* Reduces **complexity** by separating *interface* from *implementation*.
* Increases **security and maintainability** (hide sensitive or change-prone logic).
* Enables **loose coupling** — one part can change internally without breaking others.
* Forms the base of **framework and API design** (you use `List`, not `ArrayList` directly).

---

### 🧩 How Abstraction is Achieved in Java

| Mechanism          | Description                                     | Keyword     | Can Contain                                                  |
| ------------------ | ----------------------------------------------- | ----------- | ------------------------------------------------------------ |
| **Abstract Class** | A class meant to be extended, not instantiated. | `abstract`  | Abstract & concrete methods, constructors, variables         |
| **Interface**      | A contract that defines behavior.               | `interface` | Abstract, default, static (and from Java 9, private) methods |

---

## ⚙️ 1. Abstract Classes

### 🧠 Concept

An **abstract class** defines *common structure* and optionally *partial implementation* for its subclasses.

* Declared using the `abstract` keyword.
* Cannot be instantiated directly.
* Can contain:

  * **Abstract methods** → declared but not implemented.
  * **Concrete methods** → with body.
  * **Constructors** → for subclass use.
  * **Fields** → with any access modifier.

---

### 🧩 Example

```java
abstract class Vehicle {
    int speed;

    Vehicle(int speed) {
        this.speed = speed;
    }

    abstract void start(); // abstract method

    void stop() {          // concrete method
        System.out.println("Vehicle stopped.");
    }
}

class Car extends Vehicle {
    Car(int speed) {
        super(speed);
    }

    @Override
    void start() {
        System.out.println("Car starting at speed: " + speed);
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle v = new Car(100); // Upcasting
        v.start(); // Car starting...
        v.stop();  // Vehicle stopped.
    }
}
```

✅ The abstract class defines **common structure**, while subclasses define **specific behavior**.

---

### 💡 Notes

| Feature                                         | Abstract Class                   |
| ----------------------------------------------- | -------------------------------- |
| Can have constructors                           | ✅ Yes                            |
| Can have instance variables                     | ✅ Yes                            |
| Can have both abstract and non-abstract methods | ✅ Yes                            |
| Can be partially implemented                    | ✅ Yes                            |
| Multiple inheritance                            | ❌ Not allowed                    |
| Access modifiers                                | Any (public, protected, private) |

---

## ⚙️ 2. Interfaces

### 🧠 Concept

An **interface** defines a *contract* of behavior that implementing classes must fulfill.

By default (before Java 8):

* All variables are `public static final` (constants).
* All methods are `public abstract`.

From Java 8 onward:

* `default` and `static` methods allowed.
  From Java 9 onward:
* `private` methods allowed (for helper logic inside the interface).

---

### 🧩 Example

```java
interface Drawable {
    int MAX_WIDTH = 100; // implicitly public static final

    void draw();         // implicitly public abstract

    default void info() {
        System.out.println("Drawing shape...");
    }

    static void utility() {
        System.out.println("Interface static method");
    }
}

class Circle implements Drawable {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

public class Main {
    public static void main(String[] args) {
        Drawable d = new Circle();
        d.draw();
        d.info();
        Drawable.utility(); // called using interface name
    }
}
```

✅ `Circle` implements the behavior contract defined by `Drawable`.

---

### 💡 Notes

| Feature                               | Interface                                                           |
| ------------------------------------- | ------------------------------------------------------------------- |
| Variables                             | Always `public static final`                                        |
| Abstract methods                      | Implicitly `public abstract`                                        |
| Can have `default` & `static` methods | ✅ Since Java 8                                                      |
| Can have `private` methods            | ✅ Since Java 9                                                      |
| Constructors                          | ❌ Not allowed                                                       |
| Multiple inheritance                  | ✅ Supported (no ambiguity with default methods if handled properly) |

---

### ⚔️ Interface vs Abstract Class

| Feature      | Abstract Class           | Interface                                   |
| ------------ | ------------------------ | ------------------------------------------- |
| Keyword      | `abstract class`         | `interface`                                 |
| Inheritance  | Single                   | Multiple                                    |
| Constructors | Yes                      | No                                          |
| Method Types | Both concrete & abstract | Abstract, default, static (private from J9) |
| Variables    | Any                      | `public static final` only                  |
| Use Case     | Share base logic         | Define a common contract                    |

---

### ⚙️ Real-world Analogy

Imagine:

* **Interface:** “Vehicle” (what it can do — `start`, `stop`)
* **Abstract Class:** “EnginePoweredVehicle” (defines partial details)
* **Concrete Class:** “Car”, “Bike” (implements specifics)

Interfaces define **what**, abstract classes define **partial how**, and concrete classes complete the **full how**.

---

### 🧠 Tricky Interview Snippets

#### 1. Can abstract class have constructors?

✅ Yes — used by subclass constructors:

```java
abstract class Base {
    Base() { System.out.println("Base constructor"); }
}
class Derived extends Base { Derived() { System.out.println("Derived constructor"); } }
```

#### 2. Can interface have variables that change?

❌ No — they are constants (`public static final`).

#### 3. What if two interfaces have same default method?

✅ Must override to resolve ambiguity:

```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }

class C implements A, B {
    public void show() { A.super.show(); } // resolves conflict
}
```

#### 4. Can abstract class implement interface?

✅ Yes — and it may or may not implement all methods.

#### 5. Why can’t we instantiate abstract classes or interfaces?

Because they are **incomplete blueprints** — they define behavior but not a full implementation.

---

### 🧭 Summary Table

| Mechanism      | Can Have Implementation | Can Instantiate | Supports Multiple Inheritance | Contains Variables    | Can Have Constructor |
| -------------- | ----------------------- | --------------- | ----------------------------- | --------------------- | -------------------- |
| Abstract Class | ✅ Partial               | ❌               | ❌                             | Any                   | ✅                    |
| Interface      | ✅ (default/static only) | ❌               | ✅                             | `public static final` | ❌                    |

---

### 🌟 Relationship with Encapsulation & Polymorphism

* **Encapsulation** hides *data*; **Abstraction** hides *implementation logic*.
* Abstraction allows **polymorphism** — you can refer to any subclass through an abstract or interface type (`List l = new ArrayList();`).

---
