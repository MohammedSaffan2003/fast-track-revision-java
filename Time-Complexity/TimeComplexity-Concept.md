## Step 1 — Think of **time complexity as steps in a journey**

Imagine you are a **delivery person**:

* You have `n` packages to deliver.
* **Each package** takes **1 unit of time** to deliver.

```java
for (int i = 0; i < n; i++) {
    deliverPackage(i);
}
```

* How long does it take? **`n` steps** → **O(n)**.
* Story: “I visit each house one by one.”

✅ This is **linear time** — time grows **proportionally** with `n`.

---

## Step 2 — **O(1) — constant time**

Imagine you are asked:

> “Deliver the package to the house with number 42.”

* You **go straight there**, no matter how many total houses exist.
* Time doesn’t grow with `n`.

```java
deliverPackage(42);
```

* Story: “I don’t care how many houses exist, I just go to one.”

✅ This is **O(1)** — constant time.

---

## Step 3 — **O(n^2) — nested loops**

Now imagine a party: You want to **shake hands with everyone**:

```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        shakeHands(i, j);
    }
}
```

* For each person (`n`), you interact with `n` people → **n × n = n² steps**
* Story: “I greet everyone with everyone.”

✅ Quadratic time — grows **super fast** as `n` increases.

---

## Step 4 — **O(log n) — divide and conquer**

Imagine you are looking for a house **in a straight street** numbered from 1 to 100:

* You don’t check each house. You **split the street in half** each time:

  * Is my house in the first half? Go there.
  * Otherwise, go to the second half.

* Each step **cuts the search space in half** → `log n` steps.

Story: “I keep folding the street in half until I find the house.”

✅ This is **logarithmic time**, very fast.

---

## Step 5 — **O(n log n) — combination of linear and divide & conquer**

Imagine **sorting letters** in your mailbox:

* You **split them repeatedly** (like merge sort): log n levels of splitting.
* At each level, you touch all `n` letters **once** while merging.

Story: “I divide letters into piles (log n), and at each step I check every letter (n) → total n × log n.”

✅ That’s **O(n log n)** — typical in efficient sorting algorithms.

---

## Step 6 — **O(n + m) — when you have two different inputs**

Imagine you have:

* `n` packages for houses
* `m` letters for apartments

If you deliver all packages **and** all letters:

```java
for (int i = 0; i < n; i++) deliverPackage(i);
for (int j = 0; j < m; j++) deliverLetter(j);
```

* Story: “I go through each package once, and each letter once. Two different counts.”

✅ Total time = **O(n + m)**, not multiplied.

---
## **O(2^n) — exponential growth**

**Story:** Imagine you have `n` light switches.

* Each switch can be either **ON or OFF**.

* You want to **try every possible combination** of these switches.

* How many combinations? **2 × 2 × 2 … n times = 2^n**

```java
void allCombinations(int n) {
    if (n == 0) return;
    allCombinations(n-1);  // include OFF
    allCombinations(n-1);  // include ON
}
```

* Each step doubles the possibilities → **O(2^n)**
* Story: “For every switch, you split into two worlds — one ON, one OFF — and repeat.”

💡 Interviews tip: anytime your recursion **branches into 2 or more calls per input element**, think **exponential**.

---

## **O(n!) — factorial growth**

**Story:** Imagine you have **n friends** and want to **make a seating arrangement at a round table**.

* First chair: n choices

* Second chair: n-1 choices

* Third chair: n-2 choices …

* Last chair: 1 choice

* Total arrangements: **n × (n-1) × … × 1 = n!**

```java
void permute(String s, int l, int r) {
    if (l == r) print(s);
    for (int i = l; i <= r; i++) {
        swap(s, l, i);
        permute(s, l+1, r);  // recursive branching
        swap(s, l, i);
    }
}
```

* Story: “I try every seat for every friend — the number of possibilities explodes factorially.”

💡 Interviews tip: **nested recursion that permutes all elements → O(n!)**

---
### **Mnemonic story recap**

1. **O(1)** → teleport to one house.
2. **O(n)** → deliver packages one by one.
3. **O(n²)** → handshake party, everyone with everyone.
4. **O(log n)** → binary search, keep folding the street.
5. **O(n log n)** → sorting letters: divide piles (log n) × check all letters (n).
6. **O(n + m)** → deliver packages and letters separately.
7. **O(2^n)** → all combinations of n switches (branching)
8. **O(n!)** → seating arrangements for n friends

---
