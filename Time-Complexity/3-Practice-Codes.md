## **Practice Snippets**

### **1️⃣ Simple loop**

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

* **Time Complexity:** O(n)
* **Reasoning (story):** You are delivering `n` packages, one by one. Each takes 1 step.

---

### **2️⃣ Nested loops**

```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        System.out.println(i + " " + j);
    }
}
```

* **Time Complexity:** O(n²)
* **Reasoning (story):** Handshake party — each of `n` people interacts with `n` people. Steps multiply → n × n.

---

### **3️⃣ Sequential loops with different sizes**

```java
for (int i = 0; i < n; i++) doSomething();
for (int j = 0; j < m; j++) doSomethingElse();
```

* **Time Complexity:** O(n + m)
* **Reasoning (story):** Deliver `n` packages and `m` letters separately. Two independent journeys → steps add up.

---

### **4️⃣ Binary search**

```java
int left = 0, right = n - 1;
while (left <= right) {
    int mid = (left + right) / 2;
    if (arr[mid] == target) break;
    else if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
```

* **Time Complexity:** O(log n)
* **Reasoning (story):** Fold the street in half each time to find your house. Every step halves the remaining search space.

---

### **5️⃣ Merge sort (simplified)**

```java
void mergeSort(int[] arr, int l, int r) {
    if (l < r) {
        int m = (l + r) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m+1, r);
        merge(arr, l, m, r);
    }
}
```

* **Time Complexity:** O(n log n)
* **Reasoning (story):** Split letters into piles (log n levels). At each level, touch all `n` letters to merge. Multiply → n × log n steps.

---

### **6️⃣ Exponential recursion (2^n)**

```java
void allSubsets(int n) {
    if (n == 0) return;
    allSubsets(n-1);
    allSubsets(n-1);
}
```

* **Time Complexity:** O(2^n)
* **Reasoning (story):** `n` switches, each can be ON or OFF. Each choice doubles the possibilities → branching tree → 2^n steps.

---

### **7️⃣ Factorial recursion (n!)**

```java
void permute(String s, int l, int r) {
    if (l == r) System.out.println(s);
    for (int i = l; i <= r; i++) {
        swap(s, l, i);
        permute(s, l+1, r);
        swap(s, l, i);
    }
}
```

* **Time Complexity:** O(n!)
* **Reasoning (story):** Seating `n` friends in all possible arrangements. First chair: n choices, second: n-1 … total arrangements explode factorially.

---

### ✅ Quick “cheat story recap” for notes

* **O(1)** → teleport
* **O(n)** → linear delivery
* **O(n²)** → handshake party
* **O(log n)** → fold street
* **O(n log n)** → sort letters in piles
* **O(n + m)** → separate deliveries
* **O(2^n)** → all switch combinations
* **O(n!)** → seating arrangements

---
