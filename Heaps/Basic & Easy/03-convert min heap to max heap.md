

# PROBLEM:

Given an array representing a **Min Heap**, convert it **in-place** into a **Max Heap**.

Example:

```text
Input:
[3, 5, 9, 6, 8, 20, 10, 12, 18, 9]

Min Heap:
           3
        /     \
       5       9
     /  \    /   \
    6    8 20    10
   / \   /
 12 18  9

Output:
[20, 18, 10, 12, 9, 9, 3, 5, 6, 8]

Max Heap:
           20
        /      \
      18        10
     /  \      /  \
   12    9    9    3
  / \   /
 5  6  8
```

The array is already a valid heap.

We only need to **change the heap property** from

> Parent ≤ Children (Min Heap)

to

> Parent ≥ Children (Max Heap)

---

# PATTERN:

**Bottom-Up Heap Construction (Heapify Build Algorithm)**

This is the exact same pattern used in:

- Build Max Heap
- Build Min Heap
- Heap Sort
- Convert Max Heap ↔ Min Heap

---

# WHY THIS PATTERN:

Many people initially think:

> "Since it is already a heap, maybe I should repeatedly remove elements and insert them into another heap."

That works but is unnecessary.

The important observation is:

> **The tree structure never changes.**
>
> Only the ordering property changes.

So we simply rebuild the heap.

Exactly like building a heap from an arbitrary array.

---

# CORE IDEA:

Suppose we have

```text
          ?
       /      \
   Max Heap   Max Heap
```

If both children are already valid Max Heaps,

then we only need to fix the parent.

This is exactly what **heapify()** does.

Therefore:

1. Start from the last internal node.
2. Heapify it.
3. Move upward.
4. Eventually the root also becomes correct.

This is called **Bottom-Up Heap Construction**.

---

# BRUTE FORCE:

## Approach 1

Extract every element from the Min Heap.

Insert it into a Max Heap.

Finally copy back.

### Why it works

Removing from a Min Heap gives elements in increasing order.

Inserting into a Max Heap reconstructs a Max Heap.

### Complexity

Extract N elements

```
N × O(logN)
```

Insert N elements

```
N × O(logN)
```

Total

```
O(N logN)
```

Space

```
O(N)
```

---

## Is brute force important?

Usually **No.**

Interviewers directly expect the Build Heap approach.

Only mention this if they ask:

> "Can you think of another way?"

---

# OPTIMAL APPROACH:

## Idea

Use the Build Heap algorithm.

Start from

```
last non-leaf node
```

and call

```
maxHeapify()
```

until reaching the root.

---

## Why bottom-up?

Consider:

```text
          5
        /   \
      20     18
```

Suppose children are already valid Max Heaps.

Now fixing only node 5 automatically fixes the entire subtree.

If children weren't fixed first,

heapify would fail because it assumes both child subtrees are already heaps.

Hence:

```
Bottom → Top
```

not

```
Top → Bottom
```

---

# ALGORITHM:

### Step 1

Find last non-leaf node.

Formula:

```cpp
(n / 2) - 1
```

Why?

Leaves start from

```
n/2
```

So the previous node is the last parent.

---

### Step 2

Iterate backwards.

```cpp
for(i = n/2 - 1; i >= 0; i--)
```

---

### Step 3

Call

```cpp
maxHeapify(i)
```

---

### Step 4

maxHeapify compares

```
parent
left child
right child
```

Find the largest.

If parent isn't largest,

swap with largest child.

---

### Step 5 (Recursive Step)

After swapping,

the parent is fixed.

But the child where we moved the smaller value may now violate the Max Heap property.

Example:

```text
        4
      /   \
     9     8
    /
   15
```

Swap:

```text
        9
      /   \
     4     8
    /
   15
```

Now

```
4 < 15
```

Violation still exists.

So we recursively heapify that child.

This recursive call continues until:

- node becomes larger than both children
- or reaches a leaf

This is exactly why heapify is recursive.

---

# DRY RUN:

Input

```text
[3,5,9,6,8,20,10,12,18,9]
```

n = 10

Last parent

```
10/2 -1 = 4
```

Start

```
i=4
```

Heapify subtree.

---

```
i=3
```

Heapify subtree.

---

```
i=2
```

Swap

```
9 ↔ 20
```

---

```
i=1
```

Swap

```
5 ↔ 18
```

Recursive heapify fixes lower subtree.

---

```
i=0
```

Swap

```
3 ↔ 20
```

Recursive heapify continues.

Eventually

```
[20,18,10,12,9,9,3,5,6,8]
```

Done.

---

# IMPORTANT CODE SNIPPETS:

### Last parent

```cpp
int lastParent = n/2 - 1;
```

---

### Left child

```cpp
2*i+1
```

---

### Right child

```cpp
2*i+2
```

---

### Find largest

```cpp
if(left<n && arr[left]>arr[largest])
    largest=left;

if(right<n && arr[right]>arr[largest])
    largest=right;
```

---

### Swap

```cpp
swap(arr[i],arr[largest]);
```

---

### Recursive call

```cpp
heapify(arr,n,largest);
```

Remember:

We recurse because **after swapping, only that subtree may still violate the heap property**.

---

### Build Heap

```cpp
for(int i=n/2-1;i>=0;i--)
    heapify(arr,n,i);
```

This single loop converts the entire Min Heap into a Max Heap.

---

# COMMON MISTAKES:

### Mistake 1

Starting from root.

Wrong.

Heapify requires children to already be heaps.

Always go bottom-up.

---

### Mistake 2

Starting from last element.

Leaves already satisfy heap property.

Start from last **parent**.

---

### Mistake 3

Not recursively heapifying after swap.

Only the parent becomes correct.

The affected subtree may still violate the heap property.

---

### Mistake 4

Using Min Heapify instead of Max Heapify.

We are converting into a Max Heap.

---

### Mistake 5

Thinking Build Heap is O(N logN).

It is **O(N)**.

This is one of the most commonly asked interview questions.

---

# WHY I MIGHT FORGET THIS:

Because it feels like every node performs a `logN` heapify.

So people think:

```
N × logN
```

But that's incorrect.

Most nodes are leaves.

Very few nodes have large heights.

The total work across all heapify calls sums to **O(N)**.

**Reasoning:**

- About **N/2** nodes are leaves → no work.
- About **N/4** nodes have height 1.
- About **N/8** nodes have height 2.
- About **N/16** nodes have height 3.
- ...

Total work:

\[
\frac{N}{4}(1)+\frac{N}{8}(2)+\frac{N}{16}(3)+\cdots
\]

This series converges to **O(N)**.

This is why the **Build Heap algorithm runs in linear time**, even though one individual heapify call can take **O(log N)**.

Interviewers frequently ask:

> **"Why is Build Heap O(N) and not O(N log N)?"**

Be prepared to explain this reasoning.

---

# INTERVIEW FLOW:

> We are not changing the tree structure, only the heap property.

> Since heapify assumes child subtrees are already valid heaps, we must process nodes from the last internal node up to the root.

> For every node, we perform Max Heapify.

> If a swap occurs, only that affected subtree can become invalid, so we recursively heapify that subtree.

> After reaching the root, every subtree satisfies the Max Heap property, so the entire tree becomes a Max Heap.

If the interviewer asks:

**"Why recursion?"**

Answer:

> After swapping, the larger child moves up, but the smaller value moves down into one subtree. That subtree may no longer satisfy the Max Heap property, so we recursively fix only that subtree.

If the interviewer asks:

**"Why bottom-up?"**

Answer:

> Heapify works correctly only if the left and right subtrees are already heaps. Bottom-up guarantees this invariant before processing a parent.

---

# TIME COMPLEXITY:

## maxHeapify()

Worst case:

The element moves from root of a subtree to its deepest leaf.

Height of heap:

```
log N
```

So,

```
O(logN)
```

---

## Build Heap

Although we call heapify on approximately `N/2` nodes, **not every heapify travels `log N` levels**.

- Leaves do no work.
- Nodes near the bottom move only 1–2 levels.
- Only a few nodes near the top can move `log N` levels.

The total work across all heapify calls is:

\[
\sum_{h=0}^{\log N} \frac{N}{2^{h+1}} \cdot h = O(N)
\]

Hence:

**Overall Time Complexity:**

```
O(N)
```

---

# SPACE COMPLEXITY:

Ignoring recursion stack:

```
O(1)
```

Recursive stack depth equals the height of the heap:

```
O(logN)
```

So,

- **Auxiliary Space (iterative view):** `O(1)`
- **Including recursion stack:** `O(log N)`

Mention this distinction in interviews if asked.

---

# EDGE CASES:

- Empty array
- One element
- Two elements
- Already a Max Heap
- Duplicate values
- Negative values
- Complete heap with all equal elements

All work correctly.

---

# PATTERN RECOGNITION:

Whenever you hear:

- Convert Min Heap → Max Heap
- Convert Max Heap → Min Heap
- Build Heap
- Heap Sort
- Heapify entire tree
- Restore heap after changing many elements

Immediately think:

> **Bottom-Up Heap Construction**

Start from:

```cpp
n/2 -1
```

Then repeatedly call:

```cpp
heapify()
```

This is the standard Build Heap algorithm.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    void maxHeapify(vector<int>& arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest])
            largest = left;

        if (right < n && arr[right] > arr[largest])
            largest = right;

        if (largest != i) {
            swap(arr[i], arr[largest]);

            // The swapped element may violate the heap property
            // in the affected subtree, so fix that subtree.
            maxHeapify(arr, n, largest);
        }
    }

    void convertMinToMaxHeap(vector<int>& arr, int n) {
        // Start from the last non-leaf node
        for (int i = n / 2 - 1; i >= 0; i--) {
            maxHeapify(arr, n, i);
        }
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `int largest = i;`

Assume the parent is the largest initially.

---

### `left = 2*i + 1`

Index of the left child in an array-based heap.

---

### `right = 2*i + 2`

Index of the right child.

---

### Compare with left child

```cpp
if(left<n && arr[left]>arr[largest])
```

If the left child is larger, it should become the parent.

---

### Compare with right child

```cpp
if(right<n && arr[right]>arr[largest])
```

Ensure we find the largest among all three nodes.

---

### Swap

```cpp
swap(arr[i], arr[largest]);
```

Place the largest element at the root of the subtree.

---

### Recursive heapify

```cpp
maxHeapify(arr,n,largest);
```

Only the subtree where the smaller element moved can now violate the heap property, so recursively fix **that subtree only**.

---

### Build Heap loop

```cpp
for(int i=n/2-1;i>=0;i--)
```

Process parents from bottom to top so that when we heapify a node, its children are already valid Max Heaps.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Bottom-Up Build Heap
- **Heap used:** Max Heap (because the target is a Max Heap)
- **Tree structure:** Never changes
- **Start from:** `n/2 - 1` (last non-leaf node)
- **Process:** Bottom → Top
- **Heapify:** Use **Max Heapify**
- **Why recursion?** After a swap, only the affected subtree may still violate the heap property.
- **Overall Time:** **O(N)** (Build Heap)
- **Auxiliary Space:** `O(1)` (or `O(log N)` including recursion stack)

> **Interview mantra:** *"Don't rebuild the tree. Rebuild the heap property using bottom-up heapify."*
````
