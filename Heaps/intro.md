
# Heap Notes (Complete Revision)

---

# 1. What is a Heap?

A **Heap** is a specialized tree-based data structure that satisfies **two properties**:

1. It is a **Complete Binary Tree**.
2. It follows the **Heap Order Property**.

Unlike a **Binary Search Tree (BST)**, a heap is designed for **efficient access to the highest or lowest priority element**, not efficient searching.

---

# 2. Complete Binary Tree

A **Complete Binary Tree** is a binary tree where:

- Every level is completely filled except possibly the last level.
- The last level is filled from **left to right** without gaps.

### Example (Complete)

```
        10
      /    \
     8      6
    / \    /
   4   5  2
```

### Example (Not Complete)

```
        10
      /    \
     8      6
            \
             2
```

### Why is this important?

Because a Complete Binary Tree can be efficiently stored inside an **array** without using pointers.

---

# 3. Heap Property

There are two types of heaps.

## Max Heap

Every parent is **greater than or equal to** its children.

```
        100
      /     \
     90      80
    /  \
   40   50
```

- Root always contains the **largest element**.
- Used when repeatedly finding or removing the **maximum** element.

---

## Min Heap

Every parent is **smaller than or equal to** its children.

```
        1
      /   \
     3     5
    / \
   8  10
```

- Root always contains the **smallest element**.
- Used when repeatedly finding or removing the **minimum** element.

---

# 4. Important Observation

A Heap only guarantees the relationship between a **parent and its children**.

It **does NOT** guarantee ordering between siblings or different subtrees.

Example:

```
        100
      /     \
     70      90
```

This is a valid Max Heap even though **70 < 90**.

---

# 5. Heap Representation using Array

Example:

```
        50
      /    \
    30      40
   / \     / \
 10 20   35 25
```

Stored as:

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|------|---|---|---|---|---|---|---|
| Value | 50 | 30 | 40 | 10 | 20 | 35 | 25 |

No pointers are required.

---

# 6. Parent and Child Formulas

For any node at index `i`:

### Left Child

```cpp
2*i + 1
```

### Right Child

```cpp
2*i + 2
```

### Parent

```cpp
(i-1)/2
```

(Integer Division)

> **Memorize these formulas.**

---

# 7. Basic Heap Operations

## Peek

Returns the top element.

**Time Complexity:** `O(1)`

---

## Insert

1. Insert at the last position.
2. Compare with parent.
3. Move upward if Heap Property is violated.

This process is called **Heapify Up**.

**Time Complexity:** `O(log n)`

---

## Delete Top Element

1. Swap root with last element.
2. Delete the last element.
3. Move the new root downward until Heap Property is restored.

This process is called **Heapify Down**.

**Time Complexity:** `O(log n)`

---

## Build Heap

Converts an unsorted array into a heap.

**Time Complexity:** `O(n)`

> **Important Interview Fact:** Building a Heap is **O(n)**, **NOT** `O(n log n)`.

---

# 8. Heapify Up

Used during **Insertion**.

### Steps

1. Insert at the last position.
2. Compare with parent.
3. Swap if Heap Property is violated.
4. Continue until:
   - Root is reached, or
   - Heap Property becomes valid.

Maximum swaps = Height of the tree.

**Time Complexity:** `O(log n)`

---

# 9. Heapify Down

Used during **Deletion**.

### Steps

1. Swap root with last element.
2. Delete last element.
3. Compare root with children.
4. Swap with the appropriate child.
5. Continue until Heap Property becomes valid.

**Time Complexity:** `O(log n)`

---

# 10. Time Complexities

| Operation | Complexity |
|-----------|------------|
| Peek | `O(1)` |
| Insert | `O(log n)` |
| Delete | `O(log n)` |
| Heapify | `O(log n)` |
| Build Heap | `O(n)` |
| Search | `O(n)` |

> **Remember:** Heap is **NOT** good for searching.

---

# 11. Heap vs Binary Search Tree

| Heap | Binary Search Tree |
|------|---------------------|
| Optimized for Min/Max retrieval | Optimized for Searching |
| Search = `O(n)` | Search = `O(log n)` (Balanced BST) |
| Root is always Min/Max | Root has no special guarantee |
| Only Parent-Child ordering | Entire tree is ordered |

---

# 12. C++ STL Priority Queue

## Max Heap (Default)

```cpp
priority_queue<int> pq;
```

### Functions

Insert

```cpp
pq.push(x);
```

Top

```cpp
pq.top();
```

Delete Top

```cpp
pq.pop();
```

Size

```cpp
pq.size();
```

Check Empty

```cpp
pq.empty();
```

---

## Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

> **Memorize this syntax.**

---

# 13. Why Do We Use Heap?

Heap is useful when we need repeated access to:

- Largest element
- Smallest element
- Highest priority
- Lowest priority
- Top K elements
- Streaming minimum/maximum
- Efficient insertion and deletion

Instead of sorting every time, a Heap dynamically maintains the required order.

---

# 14. When Should You Think of Heap?

Whenever the problem contains words like:

- Largest
- Smallest
- Highest
- Lowest
- Maximum
- Minimum
- Top K
- K Largest
- K Smallest
- Closest
- Most Frequent
- Priority
- Repeated Extraction
- Merge K Sorted Lists
- Running Median
- Scheduling
- Stream Processing
- Dynamic Maximum
- Dynamic Minimum

> **Immediately consider using a Heap.**

---

# 15. Choosing the Correct Heap

| Problem | Heap |
|----------|------|
| Need maximum repeatedly | Max Heap |
| Need minimum repeatedly | Min Heap |
| K Largest Elements | Min Heap (size K) |
| K Smallest Elements | Max Heap (size K) |
| Running Median | Two Heaps |
| Merge K Sorted Lists | Min Heap |
| Scheduling / Priority | Priority Queue (Heap) |

---

# 16. Common Heap Problem Patterns

## Pattern 1: Top K Elements

Examples:

- Top K Frequent Elements
- K Largest Elements
- K Closest Points
- K Closest Numbers

**Usually:** Min Heap of size K.

---

## Pattern 2: K Smallest Elements

Usually solved using a **Max Heap** of size K.

---

## Pattern 3: Merge K Sorted Lists

Use **Min Heap**.

---

## Pattern 4: Repeated Maximum

Use **Max Heap**.

---

## Pattern 5: Repeated Minimum

Use **Min Heap**.

---

## Pattern 6: Scheduling Problems

Examples:

- CPU Scheduling
- Meeting Rooms
- Task Scheduler
- Priority Scheduling

Use **Heap**.

---

## Pattern 7: Streaming Problems

Examples:

- Median Finder
- Running Median

Use **Two Heaps**.

---

# 17. General Problem Solving Framework

Whenever you see a Heap problem, ask:

### Question 1

What value needs to be retrieved repeatedly?

- Maximum?
- Minimum?
- Top K?
- Highest Priority?

---

### Question 2

Should I maintain the **entire dataset** or only **K elements**?

Many Top K problems only require maintaining **K** elements.

---

### Question 3

Should I use a **Max Heap** or **Min Heap**?

Choose based on what should always remain at the root.

---

### Question 4

What should each Heap node store?

Sometimes just an integer.

Sometimes a pair:

- `(value, frequency)`
- `(distance, point)`
- `(value, index)`
- `(row, column)`
- `(time, task)`

Store whatever information is needed to compare and retrieve efficiently.

---

# 18. Common Mistakes

- Confusing Heap with BST.
- Forgetting Parent/Child formulas.
- Using the wrong Heap type.
- Using sorting instead of Heap for repeated operations.
- Forgetting that C++ `priority_queue` is a Max Heap by default.
- Thinking Heap supports binary search.
- Not limiting Heap size in Top K problems.
- Forgetting Build Heap is `O(n)`.

---

# 19. Important Interview Facts

- Heap is a **Complete Binary Tree**.
- Heap is usually implemented using an **array**.
- Priority Queue is implemented using a Heap.
- Root always contains the highest-priority element.
- Heap is optimized for insertion and deletion.
- Heap is **NOT** optimized for searching.
- Building a Heap from an array takes **O(n)**.
- Insertion uses **Heapify Up**.
- Deletion uses **Heapify Down**.
- C++ `priority_queue` is a **Max Heap** by default.

---

# 20. Interview Checklist

Before moving to Heap problems, make sure you can answer:

- ✅ What is a Heap?
- ✅ Why is it stored as an array?
- ✅ What is a Complete Binary Tree?
- ✅ Difference between Max Heap and Min Heap?
- ✅ Difference between Heap and BST?
- ✅ Parent, Left Child and Right Child formulas?
- ✅ How does Heapify Up work?
- ✅ How does Heapify Down work?
- ✅ Why is Build Heap `O(n)`?
- ✅ Time complexity of every operation?
- ✅ How to declare Max Heap and Min Heap in C++?
- ✅ When should you choose a Heap over sorting?
- ✅ How do you identify Heap problems in interviews?

---

# Final Mental Model

Think of a Heap as a **priority-based data structure**.

Its only purpose is to efficiently maintain and repeatedly retrieve the **highest-priority** or **lowest-priority** element while supporting fast insertions and deletions.

> **Whenever a problem involves repeatedly accessing the minimum, maximum, Top K, priority-based processing, or streaming data, a Heap should be one of the first data structures you consider.**
````
