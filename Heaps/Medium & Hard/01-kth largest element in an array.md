
# Kth Largest Element (Heap)

## PROBLEM:

Given an array `arr[]` and an integer `k`, find the **kth largest element** in the array.

### Example

```text
arr = [3,5,4,2,9]
k = 3

Largest order:
9 5 4 3 2

Answer = 4
```

> **Important:** Unless the problem explicitly says **"kth distinct largest"**, duplicates are counted.

Example:

```text
arr = [5,5,4]
k = 2

Answer = 5
```

---

# PATTERN:

**Top K Elements using a Fixed-Size Min Heap**

---

# WHY THIS PATTERN:

Whenever you hear problems like:

- Kth Largest
- Kth Smallest
- Top K Elements
- K Closest Elements
- K Most Frequent Elements

immediately think:

> **"I don't need all the elements. I only need the best K elements."**

Sorting the entire array gives unnecessary information.

Instead, we maintain only the **best K elements** while traversing the array.

This is exactly what a **fixed-size heap** is designed for.

---

# CORE IDEA:

Maintain the **K largest elements seen so far**.

Among these K elements,

the **smallest** one is exactly the kth largest.

Therefore, we use a **Min Heap**.

The heap always stores only the current Top K largest elements.

Whenever a larger element arrives,

we remove the smallest among the current Top K.

At the end,

the smallest element inside the heap is the answer.

---

## Why is a Heap the correct data structure?

For every new element, we need to quickly answer:

> **"Which element should be removed?"**

The answer is always:

> **The smallest among the current Top K.**

A Min Heap supports:

- Insert → **O(log K)**
- Remove Smallest → **O(log K)**
- Get Smallest → **O(1)**

making it the perfect choice.

---

## Why Min Heap and NOT Max Heap?

Suppose the current Top 3 are:

```text
9
5
4
```

A new element arrives:

```text
7
```

Top 3 become:

```text
9
7
5
```

Which element leaves?

```text
4
```

Notice:

Whenever a better element comes,

we always remove the **smallest** among the Top K.

Therefore,

the smallest must always be available immediately.

That is exactly what a **Min Heap** provides.

---

## What is stored inside the Heap?

Only integers.

More specifically,

the **K largest elements processed so far**.

It **does not** store all elements.

---

## Why do we push every element?

Every new element is a candidate for the Top K.

We don't know whether it belongs until we compare it with the existing Top K.

Therefore,

every element is inserted once.

---

## Why do we pop?

After insertion,

the heap may temporarily contain **K+1** elements.

Since we only want the Top K,

we remove the smallest.

That smallest can never become the kth largest anymore.

---

## What invariant does the Heap maintain?

After processing every element,

the heap always contains:

> **Exactly the K largest elements seen so far.**

This property remains true throughout the algorithm.

---

## Why is maintaining only K elements sufficient?

Suppose

```text
Current Top 3

20
18
15
```

A new element arrives.

### Case 1

```text
5
```

It is too small.

It gets removed immediately.

---

### Case 2

```text
25
```

Now Top 3 become

```text
25
20
18
```

15 gets removed.

Therefore,

anything outside the current Top K is irrelevant.

---

# BRUTE FORCE:

## Idea

Sort the array.

The kth largest element is located at

```cpp
arr[n-k]
```

### Code

```cpp
class Solution {
public:
    int kthLargest(vector<int>& arr, int k) {

        sort(arr.begin(), arr.end());

        return arr[arr.size()-k];
    }
};
```

### Dry Run

```text
3 5 4 2 9

Sort

2 3 4 5 9

n = 5
k = 3

Answer

arr[5-3]

arr[2]

4
```

### Time Complexity

```
O(N log N)
```

### Space Complexity

```
O(1)
```

(ignoring recursion stack)

### Why Optimize?

Sorting arranges every element.

The problem only asks for one position.

We don't care about the complete ordering.

---

# OPTIMAL APPROACH:

Maintain a **Min Heap of size K**.

For every element:

- Insert it.
- If heap size exceeds K, remove the smallest.
- Continue until all elements are processed.

At the end,

the heap contains exactly the K largest elements.

The smallest among them is the kth largest.

---

# ALGORITHM:

### Step 1

Create a Min Heap.

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

### Step 2

Traverse every element.

```cpp
for(auto num : arr)
```

---

### Step 3

Insert the current element.

```cpp
pq.push(num);
```

Every element deserves one chance to enter the Top K.

---

### Step 4

If heap size becomes greater than K,

remove the smallest.

```cpp
if(pq.size() > k)
    pq.pop();
```

This restores the invariant.

---

### Step 5

After processing all elements,

return

```cpp
pq.top();
```

The top contains the smallest among the K largest elements,

which is exactly the kth largest.

---

# DRY RUN:

### Input

```text
arr = [3,5,4,2,9]

k = 3
```

Initially

```text
Heap

{}
```

---

### Process 3

Push

```text
3
```

Heap Size

```text
1
```

No pop.

Current Top Largest

```text
3
```

---

### Process 5

Push

```text
3
5
```

Heap Size

```text
2
```

No pop.

Current Top Largest

```text
5
3
```

---

### Process 4

Push

```text
3
4
5
```

Heap Size

```text
3
```

No pop.

Current Top Largest

```text
5
4
3
```

---

### Process 2

Push

```text
2
3
4
5
```

Heap Size

```text
4
```

Too many elements.

Remove smallest

```text
2
```

Heap becomes

```text
3
4
5
```

Current Top Largest

```text
5
4
3
```

---

### Process 9

Push

```text
3
4
5
9
```

Heap Size

```text
4
```

Remove smallest

```text
3
```

Heap becomes

```text
4
5
9
```

Current Top Largest

```text
9
5
4
```

Return

```text
4
```

---

# IMPORTANT CODE SNIPPETS:

## Min Heap Declaration

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

## Push Current Element

```cpp
pq.push(num);
```

---

## Maintain Heap Size

```cpp
if(pq.size() > k)
    pq.pop();
```

---

## Return Answer

```cpp
return pq.top();
```

---

## Heap Declaration Explained

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Break it into three parts.

### 1. `int`

The data type stored inside the heap.

```cpp
priority_queue<int,...>
```

Stores integers.

---

### 2. `vector<int>`

The underlying container.

The heap is internally implemented using a vector.

You almost never change this.

---

### 3. `greater<int>`

Comparator.

It tells C++ that smaller elements have higher priority.

Therefore,

the smallest element always remains at the top.

Without it,

```cpp
priority_queue<int>
```

creates a Max Heap.

---

# COMMON MISTAKES:

### Mistake 1

Using a Max Heap.

Remember:

We are storing the Top K largest elements.

We need quick access to the smallest among them.

Hence,

**Min Heap.**

---

### Mistake 2

Popping when

```cpp
size == k
```

Correct condition

```cpp
size > k
```

We first insert,

then remove the extra element.

---

### Mistake 3

Thinking the heap is sorted.

It is **NOT**.

Only the root is guaranteed to be the smallest.

The remaining elements are **not sorted**.

---

### Mistake 4

Thinking duplicates are ignored.

Priority Queue stores duplicates.

Only `set` removes duplicates.

---

### Mistake 5

Returning the last inserted element.

The answer is always

```cpp
pq.top()
```

---

# WHY I MIGHT FORGET THIS:

The confusing part is choosing between Min Heap and Max Heap.

Remember this sentence:

> **"I keep the largest K elements. If a better element comes, I throw away the smallest among them."**

That immediately tells you:

**Use a Min Heap.**

---

# INTERVIEW FLOW:

### Step 1

Clarify.

> "Should duplicates be counted?"

---

### Step 2

Explain brute force.

Sort the array.

Return

```cpp
arr[n-k]
```

Complexity

```
O(N log N)
```

---

### Step 3

Optimize.

Say:

> "We don't need the complete sorted order. We only care about the Top K largest elements."

---

### Step 4

Identify the data structure.

Say:

> "Whenever a better element comes, I remove the smallest among the current Top K."

Therefore,

**Min Heap.**

---

### Step 5

Explain the invariant.

> "After processing every element, the heap always stores the K largest elements seen so far."

---

### Step 6

Write the code.

---

### Step 7

Dry run.

---

### Step 8

Explain complexities.

---

# TIME COMPLEXITY:

We process every element once.

For every element:

Push

```
O(log K)
```

Possibly one pop

```
O(log K)
```

Heap size never exceeds K.

Therefore,

```
O(N log K)
```

---

# SPACE COMPLEXITY:

The heap stores at most K elements.

Therefore,

```
O(K)
```

---

# EDGE CASES:

### K = 1

Return the maximum element.

---

### K = N

Return the minimum element.

---

### Duplicate values

Handled naturally.

Heaps allow duplicates.

---

### Negative numbers

No special handling required.

---

### Single element

Return that element.

---

# PATTERN RECOGNITION:

Whenever you hear:

- Kth Largest
- Kth Smallest
- Top K
- K Closest
- K Most Frequent
- Largest K Elements
- Smallest K Elements

Ask yourself:

> **"Do I really need all N elements?"**

If the answer is **No**,

think:

> **Fixed-Size Heap.**

Then decide:

- Keeping the **largest K** → **Min Heap**
- Keeping the **smallest K** → **Max Heap**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int kthLargest(vector<int>& arr, int k) {

        priority_queue<int, vector<int>, greater<int>> pq;

        for (int num : arr) {

            pq.push(num);

            if (pq.size() > k)
                pq.pop();
        }

        return pq.top();
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Creates a Min Heap so the smallest among the current Top K is always available.

---

```cpp
for(int num : arr)
```

Process every element exactly once.

---

```cpp
pq.push(num);
```

Every element gets one chance to become part of the Top K.

---

```cpp
if(pq.size() > k)
```

If we now have K+1 elements,

one must be removed.

---

```cpp
pq.pop();
```

Remove the smallest because it can never be part of the final Top K.

---

```cpp
return pq.top();
```

The heap contains the K largest elements.

The smallest among them is the kth largest.

---

# Explain Every Tricky Condition

### Why `pq.size() > k` and not `>= k`?

Suppose `k = 3`.

When the heap has exactly 3 elements, that's exactly what we want.

If we used `>= k`, we'd remove an element too early and end up with only 2 elements.

So we only remove when the heap temporarily grows to 4.

---

### Why push before popping?

Every incoming element deserves one chance to enter the Top K.

Only after inserting can we decide whether it belongs.

---

### Why does `pq.top()` always return the smallest?

A Min Heap maintains the heap property:

> Every parent is smaller than or equal to its children.

Because of this property, the smallest element always rises to the root.

The heap is **not sorted**—only the root is guaranteed to be the smallest.

---

# Easy-to-Remember Summary

- **Pattern:** Top K using a Fixed-Size Heap
- **Heap stores:** Current K largest elements
- **Heap type:** Min Heap
- Push every element because every element is a candidate.
- Pop when heap size exceeds K.
- Pop removes the smallest element.
- **Invariant:** Heap always contains the K largest elements processed so far.
- **Answer:** `pq.top()`
- **Time Complexity:** `O(N log K)`
- **Space Complexity:** `O(K)`

> **One-line intuition:**
>
> **Keep the K largest elements in a Min Heap. Whenever there are K+1 elements, remove the smallest. The smallest among the remaining K elements is the kth largest.**
````
