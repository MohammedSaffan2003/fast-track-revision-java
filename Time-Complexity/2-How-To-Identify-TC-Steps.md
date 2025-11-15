## 1️⃣ The main time complexities you **must know**

The ones I described earlier (TimeComplexity-Concept.md) are the ones **most commonly asked in interviews**. Here’s a quick “interview-ready” list, from fastest to slowest (approximate):

| Complexity     | Story/Intuition             | Examples                                   |
| -------------- | --------------------------- | ------------------------------------------ |
| **O(1)**       | Teleport to one house       | Access array element, hash map lookup      |
| **O(log n)**   | Fold street in half         | Binary search, searching in BST            |
| **O(n)**       | Deliver packages one by one | Simple loop over array/list                |
| **O(n log n)** | Sort letters in piles       | Merge sort, heap sort, quicksort (average) |
| **O(n²)**      | Handshake party             | Nested loops, bubble sort                  |
| **O(n³)**      | 3 nested loops              | Matrix multiplication brute-force          |
| **O(2^n)**     | Exponential growth          | Recursive Fibonacci, subset generation     |
| **O(n!)**      | Factorial growth            | Traveling salesman brute-force             |

> Tip: anything worse than O(n log n) is usually “not efficient” for large n.

---

## 2️⃣ Step-by-step trick to **see complexity in code**

Here’s a **mental checklist you can use during interviews**:

---

### Step A — Count loops

* **Single loop over n elements** → O(n)
* **Two nested loops over n elements** → O(n²)
* **Loop over n then loop over m** → O(n × m)
* **Sequential loops** → O(n + m)

**Mnemonic:** *“Nested multiplies, sequential adds.”*

---

### Step B — Check recursion

* **Each call splits data in half** → O(log n) per level
* **Each call does linear work** → O(n) per level → O(n log n) total
* **Each call calls itself twice for n** → O(2^n)

> Story: recursion is like a tree; each level multiplies by the number of calls.

---

### Step C — Ignore constants

* O(2n) → O(n)
* O(n + 100) → O(n)
* O(3n² + 5n) → O(n²)

> **Why?** Big-O is about **growth rate**, not exact steps.

---

### Step D — Combine multiple parts

* Loops + recursion: multiply
* Separate loops: add
* Nested recursion: multiply

---

### Step E — Watch data structures

* HashMap lookup → O(1) average
* TreeMap lookup → O(log n)
* Queue or Stack push/pop → O(1)

> Often interviewers hide complexity behind data structures, so **always ask or assume standard behavior**.

---

## 3️⃣ Quick **visual trick**

When looking at code:

1. **Count the loops**

   * `for` inside `for` → multiply → n²
   * `for` then `for` separate → add → n + m
2. **Look for recursion**

   * Is it splitting input? → log n
   * Is it doubling work? → 2^n
3. **Check function calls**

   * Is it calling another function with a loop? Multiply complexities.
4. **Ignore constants**

   * Focus on n, m, log n, etc.

---
