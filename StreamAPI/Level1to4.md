Level 1 notes
# ✅ Java Stream API — Level 1 Notes (Basics)

### 🎯 Goal:

Understand and apply the **core Stream operations**:

* `filter()`, `map()`, `forEach()`, `collect()`
* Learn the **Stream pipeline model**
* Write simple but real-world data processing logic

---

## 💡 Core Concepts

| Concept                     | Description                              | Example                                      |
| --------------------------- | ---------------------------------------- | -------------------------------------------- |
| **Stream**                  | A pipeline of data operations            | `list.stream()`                              |
| **Intermediate Operations** | Transform or filter the stream           | `.filter(...)`, `.map(...)`, `.distinct()`   |
| **Terminal Operations**     | Final operation that triggers processing | `.forEach(...)`, `.collect(...)`, `.count()` |
| **Immutability**            | Original data is not changed             | Stream creates a new result                  |

---

## 🔧 Tools Learned in Level 1

| Method                          | Purpose                                | Example                        |
| ------------------------------- | -------------------------------------- | ------------------------------ |
| `.stream()`                     | Convert collection to stream           | `names.stream()`               |
| `.filter(Predicate)`            | Keep elements that match a condition   | `x -> x % 2 == 0`              |
| `.map(Function)`                | Transform each element                 | `String::toUpperCase`          |
| `.forEach(Consumer)`            | Do something with each element         | `System.out::println`          |
| `.collect(Collectors.toList())` | Gather stream into a list              | `collect(Collectors.toList())` |
| `.collect(Collectors.toSet())`  | Gather into a set (removes duplicates) | `collect(Collectors.toSet())`  |
| `.distinct()`                   | Remove duplicates from a stream        | `.distinct()`                  |

---

## ✅ Tasks You Completed

| Task                                           | Concepts Practiced              |
| ---------------------------------------------- | ------------------------------- |
| Print even numbers                             | `.filter()`, `.forEach()`       |
| Print name lengths                             | `.map()`, string formatting     |
| Print long names in uppercase                  | Chaining `.filter()` + `.map()` |
| Store filtered names into a list               | `.collect(Collectors.toList())` |
| Remove duplicates using a Set                  | `.collect(Collectors.toSet())`  |
| Bonus: tried `.distinct()` and debugged errors | Great JShell practice!          |

---

## ⚠️ Common Errors & Fixes

| Issue                  | What Happened                           | Fix                                 |
| ---------------------- | --------------------------------------- | ----------------------------------- |
| Forgot `.collect(...)` | You had a `Stream`, but wanted a `List` | Add `.collect(Collectors.toList())` |
| Type mismatch on `Set` | Tried to assign `Set` result to `List`  | Use correct type: `Set<String>`     |

---

## 💬 Style Tips

* ✅ Prefer **method references** (`String::toUpperCase`) when possible
* ✅ Use **lambdas** (`s -> s.length() > 4`) for custom logic
* ✅ Chain multiple operations for clean, readable pipelines

---

## 📌 Quick Revision Patterns

```java
// Filter even numbers
nums.stream().filter(x -> x % 2 == 0).forEach(System.out::println);

// Map names to their lengths
names.stream().map(s -> s.length()).forEach(System.out::println);

// Filter and transform to uppercase
names.stream().filter(s -> s.length() > 4)
              .map(String::toUpperCase)
              .forEach(System.out::println);

// Store result in a list
List<String> result = names.stream()
                           .filter(...)
                           .map(...)
                           .collect(Collectors.toList());

// Remove duplicates
Set<String> unique = names.stream().collect(Collectors.toSet());

// Alternative: distinct() + collect
List<String> uniqueList = names.stream().distinct().collect(Collectors.toList());
```
Level 2 notes
Summary notes
# 📒 Level 2 Notes — Collectors & Aggregation

---

## 1. Core Collectors You Need to Know

| Collector                                           | Purpose                                              | Notes                                                               |
| --------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| **`Collectors.toList()`**                           | Collect stream elements into a `List`                | Most common collector                                               |
| **`Collectors.toSet()`**                            | Collect stream elements into a `Set` (no duplicates) | Useful to remove duplicates                                         |
| **`Collectors.toMap(keyMapper, valueMapper)`**      | Create a `Map` from stream elements                  | Must handle duplicate keys with **merge function**                  |
| **`Collectors.joining(delimiter, prefix, suffix)`** | Join strings into a single string                    | Only works on `Stream<String>`, use `.map()` to convert other types |
| **`Collectors.groupingBy(classifier, downstream)`** | Group elements by a classifier key                   | Downstream collector transforms or aggregates grouped items         |
| **`Collectors.counting()`**                         | Count the number of elements in a group              | Often combined with `groupingBy`                                    |
| **`Collectors.mapping(mapper, downstream)`**        | Transform elements in a group                        | Handy to extract fields when grouping                               |

---

## 2. Important Concepts & Tips

* **`toMap()` requires a merge function** if duplicate keys are possible, otherwise it throws `IllegalStateException`.
  Example merge function: `(existing, replacement) -> existing + ", " + replacement`

* **`joining()` only works on `Stream<String>`**. For other types (including `Map` entries), first convert elements to strings with `.map()`.

* When sorting `Map.Entry<K,V>`, **provide a comparator explicitly** (e.g., `Comparator.comparing(Map.Entry::getKey)`) because `Map.Entry` is not `Comparable`.

* `groupingBy()` can accept a **downstream collector** to further transform grouped data (like mapping, counting, or collecting to sets).

* `mapping()` is useful to extract or transform values within groups, e.g., to get just the names from grouped objects.

---

## 3. Common Mistakes & How You Solved Them

* **Mistake:** Tried to sort `Map.Entry` without comparator → **ClassCastException**
  **Fix:** Use `.sorted(Comparator.comparing(e -> e.getKey()))` or sort after `.map(...)` to strings.

* **Mistake:** Trying to `joining()` non-String streams without mapping → Use `.map(...)` first to convert elements.

* **Mistake:** Confused about downstream collectors in `groupingBy()` → Learned to use `Collectors.mapping()` to extract fields, and `Collectors.counting()` to count grouped elements.

* **Mistake:** Thought `counting()` needed extra arguments — it does not.

---

## 4. Handy Example Patterns

### To Map with merge function:

```java
.stream()
.collect(Collectors.toMap(
  keyMapper,
  valueMapper,
  (existing, replacement) -> existing + ", " + replacement
));
```

### To join strings:

```java
.stream()
.map(...)
.collect(Collectors.joining(", ", "[", "]"));
```

### Group by key and collect names:

```java
.stream()
.collect(Collectors.groupingBy(
  obj -> obj.getKey(),
  Collectors.mapping(obj -> obj.getName(), Collectors.toList())
));
```

### Group by key and count items:

```java
.stream()
.collect(Collectors.groupingBy(
  obj -> obj.getKey(),
  Collectors.counting()
));
```

---

## 5. Summary:

* **Level 2 is about mastering collectors that turn streams into useful collections and aggregated data.**
* You practiced **toMap, joining, groupingBy, mapping, counting** — all core to real-world Java stream processing.
* Remember to **handle duplicates** with merge functions, and to **transform types explicitly** when needed.
* Sorting complex objects requires comparators.
* Nested collectors (downstream collectors) are your friends for flexible grouping
Tasks and their code
# 📚 Completed Tasks — Level 1 & Level 2

---

## 🧩 Level 1: Stream Basics

### Task 1 — Filter even numbers from list

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
// Output:
// 2
// 4
// 6
```

---

## 🧩 Level 2: Collectors & Aggregation

---

### Task 1 — `toMap()` — Map words to their lengths

```java
List<String> words = List.of("apple", "banana", "pear");

Map<String, Integer> wordLenMap1 = words.stream()
    .collect(Collectors.toMap(Function.identity(), c -> c.length()));
// Output: {banana=6, apple=5, pear=4}

Map<String, Integer> wordLenMap2 = words.stream()
    .collect(Collectors.toMap(c -> c, c -> c.length()));
// Output: {banana=6, apple=5, pear=4}
```

---

### Task 2 — `toMap()` — Map user ID to name

```java
record User(int id, String name) {}
List<User> users = List.of(
    new User(1, "Alice"),
    new User(2, "Bob"),
    new User(3, "Charlie")
);

Map<Integer, String> userIdNameMap = users.stream()
    .collect(Collectors.toMap(c -> c.id(), c -> c.name()));
// Output: {1=Alice, 2=Bob, 3=Charlie}
```

---

### Task 3 — `toMap()` with merge function for duplicate keys

```java
List<String> cities = List.of("London", "Los Angeles", "Lisbon", "Lima", "Bangalore", "Belur", "Antarctica");

Map<String, String> result = cities.stream()
    .collect(Collectors.toMap(
        c -> c.substring(0, 1),
        c -> c,
        (existing, replacement) -> existing + ", " + replacement
    ));
// Output: {A=Antarctica, B=Bangalore, Belur, L=London, Los Angeles, Lisbon, Lima}
```

---

### Task 4 — `joining()` — Join names with commas

```java
List<String> names = List.of("Ravi", "Sita", "Geeta");

String joined1 = names.stream()
    .collect(Collectors.joining(", "));
// Output: "Ravi, Sita, Geeta"

String joined2 = names.stream()
    .collect(Collectors.joining(", ", "[", "]"));
// Output: "[Ravi, Sita, Geeta]"
```

---

### Task 5 — `joining()` on Map entries

```java
Map<String, Integer> wordLength = Map.of("pear", 4, "apple", 5, "banana", 6);

String joinedEntries = wordLength.entrySet().stream()
    .map(e -> e.getKey() + "=" + e.getValue())
    .collect(Collectors.joining(", "));
// Output: "pear=4, apple=5, banana=6"
```

---

### Task 6 — Sort and join entries (fix with comparator)

```java
String sortedJoinedEntries = wordLength.entrySet().stream()
    .sorted(Comparator.comparing(e -> e.getKey()))
    .map(e -> e.getKey() + "=" + e.getValue())
    .collect(Collectors.joining(", "));
// Output: "apple=5, banana=6, pear=4"
```

---

### Task 7 — `groupingBy()` — Group words by length

```java
List<String> words2 = List.of("apple", "banana", "pear", "kiwi", "mango");

Map<Integer, List<String>> wordLenGroup = words2.stream()
    .collect(Collectors.groupingBy(w -> w.length()));
// Output: {4=[pear, kiwi], 5=[apple, mango], 6=[banana]}
```

---

### Task 8 — `groupingBy()` — Group people by department (whole object)

```java
record Person(String name, String department) {}
List<Person> people = List.of(
    new Person("Alice", "HR"),
    new Person("Bob", "IT"),
    new Person("Charlie", "HR"),
    new Person("David", "IT"),
    new Person("Eve", "Finance")
);

Map<String, List<Person>> groupByDept = people.stream()
    .collect(Collectors.groupingBy(p -> p.department()));
// Output:
// {
//   Finance=[Person[name=Eve, department=Finance]],
//   HR=[Person[name=Alice, department=HR], Person[name=Charlie, department=HR]],
//   IT=[Person[name=Bob, department=IT], Person[name=David, department=IT]]
// }
```

---

### Task 9 — `groupingBy()` + `mapping()` — Group people by department with only names

```java
Map<String, List<String>> groupByDeptNames = people.stream()
    .collect(Collectors.groupingBy(
        p -> p.department(),
        Collectors.mapping(p -> p.name(), Collectors.toList())
    ));
// Output:
// {Finance=[Eve], HR=[Alice, Charlie], IT=[Bob, David]}
```

---

### Task 10 — `groupingBy()` — Group fruits by starting letter

```java
List<String> fruits = List.of("Apple", "Apricot", "Banana", "Blueberry", "Avocado");

Map<Character, List<String>> groupByFirstChar = fruits.stream()
    .collect(Collectors.groupingBy(o -> o.charAt(0)));
// Output:
// {A=[Apple, Apricot, Avocado], B=[Banana, Blueberry]}
```

---

### Task 11 — `groupingBy()` + `counting()` — Count words by length

```java
Map<Integer, Long> countByLength = words2.stream()
    .collect(Collectors.groupingBy(String::length, Collectors.counting()));
// Output: {4=2, 5=2, 6=1}
```
Level 2 - sub-extra learning points
for groupingBy()
## 🧠 Add This to Your Notes Later:

### 🧩 `groupingBy(...)` can take a *second argument*:

* This is called the **downstream collector**
* Use it to transform or aggregate the grouped items

### 🧰 Example Patterns:

```java
Collectors.groupingBy(
    keyMapper,
    Collectors.mapping(valueMapper, Collectors.toList())
)
```

🔁 Also works with other collectors like:

* `Collectors.counting()` → for group counts
* `Collectors.toSet()` → for unique items per group
* `Collectors.summingInt(...)`, `averagingInt(...)`, etc.
for joining
## 🧾 Add to Your Notes (Suggested Bullets)

Here's a mini list you can keep when reviewing this topic:

---

### 🧠 `Collectors.joining()` Recap:

* Only works on `Stream<String>`
* Use `.map(...)` to convert objects or entries to string first
* `joining(delimiter, prefix, suffix)` allows custom formatting

### 💡 Common Pitfalls:

* **Don't call `.sorted()` on `Map.Entry` without a comparator** → causes `ClassCastException`
* Sorting can be done either:

  * On the entries using `Comparator.comparing(...)`
  * Or after converting to string with `.map(...).sorted()`

### 🧰 Example Patterns:

```java
entrySet().stream()
    .map(e -> e.getKey() + "=" + e.getValue())
    .sorted() // optional
    .collect(Collectors.joining(", "))
```

DownstreamCollecors
how many are downstream, which are those and when to use
### 🧠 What is a *downstream collector*?

A **downstream collector** is just a collector used **inside** another collector — typically inside things like:

* `groupingBy`
* `partitioningBy`
* `collectingAndThen`
* `flatMapping`
* `mapping`

Think of it this way:

> When a collector (like `groupingBy`) collects **multiple items per group**, it needs a **secondary strategy** to say *“and what should I do with those items?”*

That’s where the **downstream collector** comes in.

---

### 🧾 Common downstream collectors (you'll use a lot):

Here’s a simple list of **common downstream collectors**:

| Collector                           | What it does                                      |
| ----------------------------------- | ------------------------------------------------- |
| `Collectors.toList()`               | Collects to a list                                |
| `Collectors.toSet()`                | Collects to a set                                 |
| `Collectors.toMap(...)`             | Collects to a map                                 |
| `Collectors.counting()`             | Counts elements                                   |
| `Collectors.summingInt(...)`        | Sums values                                       |
| `Collectors.averagingDouble(...)`   | Averages                                          |
| `Collectors.mapping(...)`           | Transforms elements *before* collecting           |
| `Collectors.flatMapping(...)`       | Flattens nested streams                           |
| `Collectors.reducing(...)`          | Performs reduction                                |
| `Collectors.collectingAndThen(...)` | Applies one collector, then transforms the result |

---

### ✅ Examples in action

#### 1. `groupingBy` with `toList()`:

```java
groupingBy(Person::department, toList())
```

→ groups people by department, collecting them into a list.

---

#### 2. `groupingBy` with `mapping(...)`:

```java
groupingBy(Person::department, 
    mapping(Person::name, toList()))
```

→ groups by department, **but collects only the names**.

---

#### 3. `partitioningBy` with `counting()`:

```java
partitioningBy(p -> p.age() > 30, 
    counting())
```

→ splits people into two groups (age > 30 or not), and **counts** them.

---

### 🔍 How do you know which ones are downstream?

Here’s a tip:

> **Any collector** that can be passed *into another collector* is a downstream collector.

And practically speaking:
If you see a collector method that accepts **another `Collector` as a parameter**, it’s probably expecting a downstream collector.

---

### 🧠 Practice tip:

If you ever forget which collectors are downstream, look at the method signature in your IDE.

Try this in JShell or an IDE:

```java
Collectors.groupingBy(
    Function.identity(), 
    Collectors.counting()
)
```

Your IDE will show:

```java
groupingBy(Function<T, K> classifier, Collector<? super T, A, D> downstream)
```

That second argument — `Collector` — is where downstreams go.
Level 3 notes
Initial explanation
# 🧠 Level 3: Advanced Filters, Sorting & Optional — Mini Guide

---

## 🔹 `sorted()`

Sorts the stream elements. By default:

* Works for types that implement `Comparable` (like `String`, `Integer`)
* For custom objects → pass a **comparator**

```java
list.stream().sorted(); // natural order
list.stream().sorted(Comparator.comparing(obj -> obj.salary())); // custom
```

---

## 🔹 `distinct()`

Removes duplicate elements (based on `equals()` and `hashCode()`).

```java
List.of(1, 2, 2, 3).stream().distinct(); // Output: 1, 2, 3
```

---

## 🔹 `limit(n)` & `skip(n)`

Control how many elements go through the stream:

* `limit(n)` → Keep only first `n` elements
* `skip(n)` → Ignore the first `n` elements

```java
list.stream().limit(3); // first 3 only
list.stream().skip(2);  // skip first 2
```

Can be combined:

```java
list.stream().skip(2).limit(3); // 3 elements after skipping 2
```

---

## 🔹 `Optional` and `.findFirst()`, `.max()`, `.min()`

These operations **might not return a result**, so they return `Optional<T>` instead of `T`.

### `.findFirst()`

Returns the **first** element in a stream that matches a filter — wrapped in `Optional`.

```java
Optional<String> result = names.stream()
    .filter(name -> name.startsWith("S"))
    .findFirst();
```

### `.max()` and `.min()`

Find the max or min element using a comparator:

```java
Optional<Employee> highestPaid = employees.stream()
    .max(Comparator.comparing(e -> e.salary()));
```

You can call `.get()`, `.orElse()`, or `.ifPresent()` on `Optional`
task1 notes -
### ✅ **Your Task 1 Results Recap**

**Goal:** Get top 3 highest-paid employees (descending salary)

You did:

```java
employees.stream()
    .sorted(Comparator.comparing(Employee::salary).reversed()) // Sort by salary, descending
    .limit(3)                                                  // Top 3
    .map(Employee::name)                                       // Extract names
    .collect(Collectors.toList());                             // Output: [Eve, Charlie, Alice]
```

✔️ Perfect logic and clean usage of method references.
✔️ `Comparator.comparing(...).reversed()` is the idiomatic way to sort descending in streams.

---

### 🧠 Bonus: Secondary Sorting (Tie-breaking)

You also explored this:

```java
.sorted(
    Comparator.comparing(Employee::salary).reversed()
              .thenComparing(Employee::name)
)
```

This is spot-on. It tells Java:

> “Sort by salary descending. If two salaries are equal, sort by name ascending.”

This level of control becomes crucial when working with sorted leaderboards, UI tables, or DB-like queries.

---

### 🎓 Key Learnings from Task 1:

* `sorted()` accepts a **Comparator**
* `Comparator.comparing(...).reversed()` for descending order
* Use `.limit(n)` to grab top `n` after sorting
* Use `.map(...)` to extract fields
* Use `.thenComparing(...)` for secondary/tiebreaker sort

---

### 👀 Bonus Insight You Might’ve Noticed:

In JShell:

```java
.sorted(...).limit(3).map(...).sorted(...)
```

That second `.sorted(...)` happens **after** mapping — so it’s just sorting names alphabetically, *not salaries*. That’s okay if you want to alphabetize top-3 names, but not if you care about pay ranking. Just something to be aware of in stream flow.
how to handle optional with orElse() coz get() might throw exception -
## 🔍 Quick Recap: `.findFirst()` with `Optional<T>`

* `.findFirst()` returns an **`Optional<T>`** — meaning the value might not be present
* **Safe way to access it:**

  * `.ifPresent(...)` → only runs if value exists
  * `.orElse(...)` → provides a fallback
  * `.get()` → risky if empty (throws exception)

---

### 🧠 Example fallback:

```java
String result = names.stream()
    .filter(n -> n.startsWith("Z"))
    .findFirst()
    .orElse("No match found");
```

This avoids `NoSuchElementException` if nothing is found.
Summary notes
# ✅ **Java Stream API – Level 3 Notes**

### 🎯 Goal: Learn to filter, sort, and safely extract data using advanced stream operations.

---

## 🔧 **Concepts & Methods Learned**

| Concept / Method      | What It Does                                                      |
| --------------------- | ----------------------------------------------------------------- |
| `.sorted()`           | Sorts elements (natural or using a comparator)                    |
| `.distinct()`         | Removes duplicates (based on `.equals()` and `.hashCode()`)       |
| `.limit(n)`           | Limits stream to the first `n` elements                           |
| `.skip(n)`            | Skips the first `n` elements                                      |
| `.findFirst()`        | Returns the first matching element as an `Optional<T>`            |
| `.max()` / `.min()`   | Returns max/min element using a comparator (as `Optional<T>`)     |
| `Optional<T>` methods | `.get()`, `.orElse()`, `.ifPresent(...)`, `.ifPresentOrElse(...)` |

---

## ✅ **Tasks Completed + Your Solutions**

---

### 🧪 Task 1: Get Top 3 Highest-Paid Employees

```java
employees.stream()
    .sorted(Comparator.comparing(Employee::salary).reversed())
    .limit(3)
    .map(Employee::name)
    .collect(Collectors.toList());
// Result: [Eve, Charlie, Alice]
```

🔹 Bonus:

```java
.sorted(Comparator.comparing(Employee::salary).reversed()
    .thenComparing(Employee::name)) // For secondary sorting
```

---

### 🧪 Task 2: Remove Duplicates from a List

```java
cities.stream().distinct().forEach(System.out::println);
// Result: Delhi, Mumbai, Bangalore, Chennai, Kolkata
```

---

### 🧪 Task 3: Find First Name Starting With “S”

```java
Optional<String> sname = names.stream()
    .filter(s -> s.startsWith("S"))
    .findFirst();

sname.ifPresent(System.out::println);  // Sunita
sname.get();                           // "Sunita"
```

---

### 🧪 Task 4: Handle Missing Values Safely

```java
String res = countries.stream()
    .filter(c -> c.startsWith("U"))
    .findFirst()
    .orElse("No country name starts with a U");
// Output: "No country name starts with a U"
```

Or:

```java
.findFirst().ifPresentOrElse(
    System.out::println,
    () -> System.out.println("No country found")
);
```

---

### 🧪 Task 5: Sort Products by Price

#### Ascending:

```java
products.stream()
    .sorted(Comparator.comparing(Product::price))
    .map(p -> p.name() + "=" + p.price())
    .collect(Collectors.toList());
// [Monitor=249.99, Tablet=399.99, Phone=699.99, Laptop=999.99]
```

#### Descending:

```java
products.stream()
    .sorted(Comparator.comparing(Product::price).reversed())
    .map(p -> p.name() + "=" + p.price())
    .collect(Collectors.toList());
// [Laptop=999.99, Phone=699.99, Tablet=399.99, Monitor=249.99]
```

---

## 🧠 **Extra Learnings / Mistakes & Fixes**

* ✅ Explored `.get()` vs `.orElse()` vs `.ifPresentOrElse()` with `Optional`
* ✅ Used `.limit(1)` as an alternative to `.findFirst()` (valid, but returns a list)
* ⚠️ `.sorted()` after `.map()` only sorts mapped values — not original object fields
* ✅ Used `.thenComparing()` for multi-level sorting (e.g., salary → name)



Level 4 notes
Initial explanation
# 🧩 **LEVEL 4: FlatMap & Nested Structures**

## 🔧 Tools You’ll Learn

| Tool / Concept                 | What It Does / Why It Matters                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------- |
| `flatMap()`                    | Flattens nested streams — turns `Stream<Stream<T>>` into `Stream<T>`                    |
| Nested `List<List<...>>`       | Common real-world data pattern — needs flattening to process inner items                |
| Map of Lists                   | Requires accessing both keys and inner list elements — perfect use-case for `flatMap()` |
| Chaining `map()` + `flatMap()` | Lets you transform deeply nested data step-by-step                                      |

---

## 🔍 **Concept Breakdowns + Examples**

---

### 1. **`.flatMap()` — Flatten Nested Streams**

You already know `.map()` transforms each element into another.

But what if each transformation gives you a **stream or list**?

```java
List<List<String>> words = List.of(
    List.of("apple", "banana"),
    List.of("cherry"),
    List.of("date", "fig", "grape")
);
```

### 🧠 Problem:

* `.map(List::stream)` gives `Stream<Stream<String>>` → can't work with this directly

### ✅ Solution:

* `.flatMap(List::stream)` → merges all inner lists into one stream

```java
words.stream()
     .flatMap(List::stream)
     .forEach(System.out::println); // prints all fruits in one flat list
```

---

### 2. **Handling Objects with Internal Lists**

Suppose you have:

```java
record Person(String name, List<String> phoneNumbers) {}
```

You want all phone numbers, no matter who they belong to:

```java
people.stream()
      .flatMap(person -> person.phoneNumbers().stream())
      .distinct()
      .collect(Collectors.toList());
```

✅ This flattens all `List<String>`s from each person into one combined stream of numbers.

---

### 3. **Map of Lists**

For example:

```java
Map<String, List<String>> departments = Map.of(
    "CS", List.of("Alice", "Bob"),
    "Math", List.of("Charlie"),
    "Physics", List.of("David", "Eve")
);
```

To get all students:

```java
departments.values().stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());
// Result: [Alice, Bob, Charlie, David, Eve]
```

You’re **flattening the `Collection<List<String>>` into `Stream<String>`**

---

### 4. **Chaining map() + flatMap()**

Sometimes you want to:

* `map` to get an inner object
* `flatMap` its internal list

For example:

```java
// Get all orders from all users
users.stream()
     .map(User::getOrders)         // Stream<List<Order>>
     .flatMap(List::stream)        // Stream<Order>
     .collect(Collectors.toList());
```

This is a common **functional-style pattern** for handling hierarchical data.

---

## 🧠 Recap Cheatsheet

| You Have              | You Want                    | Use                      |
| --------------------- | --------------------------- | ------------------------ |
| `List<List<T>>`       | `List<T>`                   | `.flatMap(List::stream)` |
| `Stream<List<T>>`     | `Stream<T>`                 | `.flatMap(...)`          |
| `Object with List<T>` | All inner elements combined | `.map(...).flatMap(...)` |
| `Map<K, List<V>>`     | All values `V`              | `.values().flatMap(...)` |
