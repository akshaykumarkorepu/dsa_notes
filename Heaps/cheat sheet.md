
# 📘 Heap Interview Cheat Sheet

The easiest way to identify whether to use a **Min Heap** or a **Max Heap** is to classify the problem into one of these two categories:

1. **Processing Problems**
2. **Top K Problems**

Once you identify the category, choosing the heap becomes straightforward.

---

# 1️⃣ Processing Problems

## The Rule

Ask yourself:

> **"What do I need next?"**

The heap should always give you the next element that needs to be processed.

| Need Next | Heap |
|-----------|------|
| Smallest element | ✅ Min Heap |
| Largest element | ✅ Max Heap |

### Why?

Processing problems repeatedly perform the same operation until all elements are processed.

The heap allows us to retrieve the required element in **O(log n)** after every operation.

---

## Example 1: Minimum Cost of Ropes

Ropes:

```
2 3 4 6
```

Steps:

```
Take 2 and 3
Cost = 5

Heap:
4 5 6

Take 4 and 5
Cost = 9

Heap:
6 9

Take 6 and 9
Cost = 15
```

Observation:

At every step we need the **two smallest ropes**.

Therefore:

```
Need smallest every time
↓
Min Heap
```

---

## Example 2: Huffman Coding

Frequencies:

```
5 9 12 13 16 45
```

Algorithm:

```
Take two smallest frequencies
Merge them
Insert merged frequency
Repeat
```

Observation:

```
Need smallest every iteration
↓
Min Heap
```

---

## Example 3: Optimal Merge Pattern (Merge Files)

Files:

```
10 20 30
```

Steps:

```
10 + 20 = 30

Heap:
30 30

30 + 30 = 60
```

Observation:

```
Always merge the two smallest files
↓
Min Heap
```

---

## Memory Trick

Ask:

> **What do I need next?**

```
Need smallest?
↓
Min Heap

Need largest?
↓
Max Heap
```

---

# 2️⃣ Top K Problems

These problems are different.

We are **not processing every element**.

Instead, we want to **keep only the best K elements** while scanning the array.

---

## The Rule

Ask yourself:

> **"Who should be removed when the heap size exceeds K?"**

The answer determines the heap.

---

## Case 1: Kth Largest / Top K Largest

Example:

```
K = 3

Array:

2 7 9 1 10 5
```

Goal:

```
Keep

10
9
7
```

Suppose heap contains:

```
7 9 10
```

Next element:

```
12
```

Heap temporarily becomes:

```
7 9 10 12
```

Need only K elements.

Who should leave?

```
7
```

Which is the **smallest**.

Therefore the heap top should always contain the smallest element.

```
Remove smallest
↓
Min Heap
```

---

## Case 2: Kth Smallest / Top K Smallest

Goal:

```
Keep

1
2
3
```

Suppose heap contains:

```
1 2 3
```

Next element:

```
0
```

Heap temporarily becomes:

```
0 1 2 3
```

Need only K elements.

Who should leave?

```
3
```

Which is the **largest**.

Therefore:

```
Remove largest
↓
Max Heap
```

---

## Case 3: Top K Frequent Elements

Frequencies:

```
A → 7
B → 5
C → 2
D → 1
```

Need:

```
A
B
```

Heap stores:

```
Frequency
```

When heap exceeds K:

Who should leave?

```
Lowest frequency
↓
Min Heap
```

---

## Case 4: K Closest Points to Origin

Distances:

```
A = 2
B = 5
C = 1
D = 7
```

Need:

```
1
2
```

Heap temporarily becomes:

```
1
2
7
```

Need only K elements.

Who should leave?

```
7
```

Which is the **largest distance (farthest point)**.

Therefore:

```
Remove farthest
↓
Max Heap
```

---

## Memory Trick

Ask:

> **Who should be removed when the heap size exceeds K?**

| Want to Keep | Remove | Heap |
|--------------|---------|------|
| Largest K | Smallest | ✅ Min Heap |
| Smallest K | Largest | ✅ Max Heap |
| Highest Frequency | Lowest Frequency | ✅ Min Heap |
| Closest Points | Farthest Point | ✅ Max Heap |

---

# Universal Heap Decision Flow

## Processing Problems

```
Is this a Processing Problem?

        YES
         │
         ▼
What do I need next?

Need smallest?
        │
        ▼
    Min Heap

Need largest?
        │
        ▼
    Max Heap
```

---

## Top K Problems

```
Is this a Top K Problem?

        YES
         │
         ▼
Heap size becomes K+1

Who should be removed?

Remove smallest?
        │
        ▼
    Min Heap

Remove largest?
        │
        ▼
    Max Heap
```

---

# Heap Interview Cheat Sheet

| Problem | Category | Heap | Reason |
|----------|----------|------|--------|
| Kth Largest | Top K | ✅ Min Heap | Remove the smallest among the largest K elements |
| Kth Smallest | Top K | ✅ Max Heap | Remove the largest among the smallest K elements |
| Top K Frequent Elements | Top K | ✅ Min Heap | Remove the least frequent element |
| K Closest Points to Origin | Top K | ✅ Max Heap | Remove the farthest point (largest distance) |
| Minimum Cost of Ropes | Processing | ✅ Min Heap | Always process the two smallest ropes first |
| Huffman Coding | Processing | ✅ Min Heap | Always combine the two smallest frequencies |
| Optimal Merge Pattern (Merge Files) | Processing | ✅ Min Heap | Always merge the two smallest files first |

---

# 🎯 30-Second Interview Rule

Whenever you encounter a heap problem, don't immediately think:

> **"Min Heap or Max Heap?"**

Instead, ask these two questions:

## ✅ Processing Problems

> **What do I need next?**

- Need the **smallest** → ✅ Min Heap
- Need the **largest** → ✅ Max Heap

---

## ✅ Top K Problems

> **Who should be removed when the heap size exceeds K?**

- Remove the **smallest** → ✅ Min Heap
- Remove the **largest** → ✅ Max Heap

---

# ⭐ Golden Formula

```
PROCESSING PROBLEMS
───────────────────
Need next smallest  → Min Heap
Need next largest   → Max Heap


TOP K PROBLEMS
──────────────
Keep largest K      → Remove smallest → Min Heap
Keep smallest K     → Remove largest  → Max Heap
Keep top K frequent → Remove least frequent → Min Heap
Keep K closest      → Remove farthest → Max Heap
```

> **The Two Questions to Remember**
>
> 1. **What do I need next?** → Processing Problems
> 2. **Who should be removed?** → Top K Problems
>
> If you answer these correctly, you'll almost always choose the correct heap in interviews.
````
