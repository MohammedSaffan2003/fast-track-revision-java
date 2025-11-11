### 📘 Definition

**Inheritance** is one of the core pillars of Object-Oriented Programming (OOP) in Java.
It allows a class (child/subclass) to **acquire the properties (fields)** and **behaviors (methods)** of another class (parent/superclass).

👉 It promotes **code reusability**, **modularity**, and helps represent **real-world hierarchies** (“is-a” relationship).

```java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();  // inherited method
        d.bark(); // Dog’s own method
    }
}
```

**Output:**

```
Animal is eating
Dog is barking
```

---

### 💡 Why Use Inheritance?

1. **Code Reusability:** You can reuse existing tested logic instead of rewriting it.
2. **Modularity:** Common logic stays in the parent class; specific logic goes into child classes.
3. **Extensibility:** You can easily extend existing features in new classes.
4. **Readability:** Class hierarchies represent real-world relationships clearly.

---

### 🧩 Types of Inheritance in Java

| Type             | Example                      | Supported in Java?                     | Description                                 |
| ---------------- | ---------------------------- | -------------------------------------- | ------------------------------------------- |
| **Single**       | `A -> B`                     | ✅                                      | One class inherits another                  |
| **Multilevel**   | `A -> B -> C`                | ✅                                      | A chain of inheritance                      |
| **Hierarchical** | `A -> B`, `A -> C`           | ✅                                      | Multiple subclasses inherit the same parent |
| **Multiple**     | `A, B -> C`                  | ❌ (for classes)                        | Not supported directly (causes ambiguity)   |
| **Hybrid**       | Combination using interfaces | ⚙️ Partially supported with interfaces |                                             |

---

### ⚠️ Why Multiple Inheritance is Not Supported

If Java allowed multiple inheritance, a child could inherit the same method from two parents, causing **ambiguity** (the *diamond problem*).

```java
class A {
    void show() { System.out.println("A’s show"); }
}
class B {
    void show() { System.out.println("B’s show"); }
}
// class C extends A, B {} // ❌ Compile-time error
```

✅ **Solution:** Java uses **interfaces** to achieve multiple inheritance of *type* (not implementation).

```java
interface A { void show(); }
interface B { void show(); }

class C implements A, B {
    public void show() { System.out.println("C’s own show"); }
}
```

---

### 🧱 Constructor Chaining and `super()`

When a subclass object is created:

1. The **parent constructor runs first**,
2. Then the **child constructor runs**.

`super()` is used to explicitly call the parent’s constructor, and **must be the first statement** in the child’s constructor.

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        super();  // calls Parent constructor
        System.out.println("Child constructor");
    }
}

public class Demo {
    public static void main(String[] args) {
        new Child();
    }
}
```

**Output:**

```
Parent constructor
Child constructor
```

🧩 If you don’t write `super()`, Java implicitly calls the **no-argument constructor** of the parent class.

---

### 🔗 The `super` Keyword

| Use                     | Syntax               | Description                                    |
| ----------------------- | -------------------- | ---------------------------------------------- |
| Call parent constructor | `super()`            | Must be first line in subclass constructor     |
| Access parent method    | `super.methodName()` | Useful when overridden                         |
| Access parent field     | `super.fieldName`    | When variable name is same in parent and child |

```java
class Animal {
    String color = "white";
    void eat() { System.out.println("Animal eats"); }
}

class Dog extends Animal {
    String color = "black";
    void eat() {
        super.eat(); // call parent method
        System.out.println("Dog eats bones");
        System.out.println("Parent color: " + super.color);
    }
}
```

---

### 🚫 `final` and `static` Restrictions

1. **`final` class** cannot be extended.

   ```java
   final class Vehicle {}
   class Car extends Vehicle {} // ❌ Error
   ```

2. **`final` method** cannot be overridden.

   ```java
   class A {
       final void show() {}
   }
   class B extends A {
       void show() {} // ❌ Error
   }
   ```

3. **`static` methods** are **hidden**, not overridden.

   ```java
   class A {
       static void display() { System.out.println("A"); }
   }
   class B extends A {
       static void display() { System.out.println("B"); }
   }
   A obj = new B();
   obj.display(); // prints "A" because it's class-based
   ```

---

### ⚙️ Method Overriding Rules (Detailed)

| Rule                 | Explanation                                                                 | Example                                                         |
| -------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Access Modifier**  | The overriding method cannot be *more restrictive* than the overridden one. | `public` → can’t become `protected` or `private`                |
| **Return Type**      | Can be **covariant** — the subtype of parent’s return type.                 | Parent returns `Animal`, child can return `Dog`.                |
| **Exceptions**       | Cannot throw broader *checked* exceptions.                                  | If parent throws `IOException`, child cannot throw `Exception`. |
| **Static / Private** | Not inherited, so they can’t be overridden.                                 | Only instance methods are overridden.                           |
| **Annotation**       | Always use `@Override` — ensures correctness at compile time.               | Prevents silent bugs.                                           |

#### 📘 Example:

```java
class Animal {
    public Animal getAnimal() { return new Animal(); }
    public void eat() throws IOException { System.out.println("Animal eats"); }
}

class Dog extends Animal {
    @Override
    public Dog getAnimal() { return new Dog(); } // Covariant return type

    @Override
    public void eat() throws FileNotFoundException { // Narrower checked exception
        System.out.println("Dog eats bones");
    }
}
```

---

### 🧩 `Object` Class – The Root of All Classes

Every class in Java implicitly extends `java.lang.Object`.
That’s why even if you don’t write `extends`, you still inherit its methods.

**Common inherited methods:**

* `toString()`
* `equals(Object obj)`
* `hashCode()`
* `getClass()`
* `clone()` (protected)
* `finalize()` (deprecated)

```java
class Demo {}
public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();
        System.out.println(d.toString()); // from Object class
    }
}
```

---

### 💡 Best Practices

* Use inheritance only when **“is-a”** relationship truly exists.
  e.g. `Car` **is a** `Vehicle` ✅, but `Car` **has a** `Engine` (use composition here).
* Avoid **deep inheritance hierarchies** — they reduce maintainability.
* Always use `@Override` when overriding.
* Keep parent classes general and children specialized.

---

### 🧠 Tricky Interview Snippets

#### 🔹 1. Static method hiding

```java
class A {
    static void print() { System.out.println("A"); }
}
class B extends A {
    static void print() { System.out.println("B"); }
}
A obj = new B();
obj.print(); // prints "A"
```

✅ Static methods are resolved at **compile-time** using **reference type**, not object type.

---

#### 🔹 2. Private members and inheritance

```java
class A {
    private int data = 40;
}
class B extends A {
    void show() {
        // System.out.println(data); ❌ Not accessible
    }
}
```

✅ Private members are **not inherited** directly, though accessible via public/protected getters.

---

#### 🔹 3. Method overriding + super

```java
class A {
    void display() { System.out.println("A"); }
}
class B extends A {
    void display() {
        super.display();
        System.out.println("B");
    }
}
new B().display();
```

**Output:**

```
A
B
```

✅ `super.display()` calls the parent version first.

---

#### 🔹 4. Overriding and Exception Hierarchy

```java
class Parent {
    void test() throws IOException {}
}
class Child extends Parent {
    void test() throws Exception {} // ❌ Compile-time error
}
```

✅ You can’t throw a **broader checked exception** in the child method.

---

#### 🔹 5. Constructor Execution Order

```java
class A {
    A() { System.out.println("A"); }
}
class B extends A {
    B() { System.out.println("B"); }
}
class C extends B {
    C() { System.out.println("C"); }
}
new C();
```

**Output:**

```
A
B
C
```

✅ Parent constructors always execute **before** child constructors.

---

### 🧭 Summary Table

| Concept              | Keyword              | Behavior                           |
| -------------------- | -------------------- | ---------------------------------- |
| Inheritance          | `extends`            | Reuse and extend functionality     |
| Constructor chaining | `super()`            | Parent constructor runs first      |
| Access parent        | `super.field/method` | Calls parent’s version             |
| Stop inheritance     | `final`              | Prevents subclassing or overriding |
| Reuse logic          | “is-a” hierarchy     | Proper OOP modeling                |

---
