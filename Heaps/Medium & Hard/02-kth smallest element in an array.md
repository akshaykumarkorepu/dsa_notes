

## PROBLEM:

Given an integer array `arr[]` and an integer `k`, find the **kth smallest element** in the array.

> The kth smallest element is determined according to the sorted order of the array.

---

# PATTERN:

**Top K Elements using a Fixed-Size Max Heap**

---

# WHY THIS PATTERN:

We only need the **k smallest elements**, not the entire sorted array.

Instead of sorting all `n` elements (`O(n log n)`), we maintain only the smallest `k` elements seen so far.

A heap allows us to efficiently:

- Insert a new candidate.
- Remove the unnecessary element.
- Always know the current kth smallest.

This reduces the complexity to:

```
O(n log k)
```

which is significantly better when `k << n`.

---

# CORE IDEA:

Maintain a **Max Heap of size k**.

The heap always stores the **k smallest elements seen so far**.

Among these `k` elements, the **largest** stays at the top.

Whenever a smaller element arrives:

- Insert it into the heap.
- If the heap size exceeds `k`, remove the largest element.

At the end,

```
Heap = k smallest elements

Top of Heap = largest among them

= kth smallest element
```

---

# BRUTE FORCE:

## Approach: Sort the array

Sort the array in ascending order.

Return:

```cpp
arr[k-1]
```

### Code

```cpp
sort(arr.begin(), arr.end());
return arr[k-1];
```

### Time Complexity

```
O(n log n)
```

Sorting every element is unnecessary since we only need one position.

### Space Complexity

```
O(1)
```

(assuming in-place sorting like IntroSort used by C++ STL)

---

# OPTIMAL APPROACH:

Use a **Fixed-Size Max Heap**.

---

## Why Heap?

A heap allows us to efficiently maintain the best `k` candidates while processing the array in one pass.

Each insertion or deletion takes only:

```
O(log k)
```

instead of sorting everything.

---

## Why Max Heap?

We want to keep the **k smallest elements**.

Whenever a new smaller element appears, we need to remove the **largest** among the current `k` smallest.

A Max Heap gives:

- Largest element in `O(1)`
- Remove largest in `O(log k)`

Exactly what we need.

---

## Why NOT Min Heap?

A Min Heap keeps the smallest element on top.

But we never want to remove the smallest.

We want to remove the **largest among the smallest k elements**.

Therefore:

- kth Smallest → Max Heap
- kth Largest → Min Heap

---

## What is stored in the Heap?

Only integers.

Specifically,

```
The current k smallest elements encountered so far.
```

---

## Why are elements pushed?

Every new element is a possible candidate for the smallest `k`.

So every element is inserted.

---

## Why are elements popped?

If the heap size exceeds `k`, then one element must be removed.

The element removed is the **largest**, because it cannot belong to the smallest `k` anymore.

---

## Heap Invariant (Most Important)

At every iteration:

- Heap size is exactly `k` (after adjustment).
- Heap contains the **k smallest elements seen so far**.
- Heap top is the **largest among these k elements**.

Therefore,

```
Heap Top = kth smallest element till now.
```

After processing the entire array,

```
Heap Top = kth smallest element of the array.
```

---

## Why Maintaining Only K Elements is Sufficient?

Suppose:

```
k = 4
```

If we already know the smallest four elements,

then any element larger than the largest among these four can never become one of the smallest four.

So keeping extra elements serves no purpose.

This is the essence of the **Top K Heap Pattern**.

---

# ALGORITHM:

1. Create an empty Max Heap.
2. Traverse every element.
3. Push the current element into the heap.
4. If heap size becomes greater than `k`, remove the top element.
5. Continue until all elements are processed.
6. Return the top of the heap.

---

# DRY RUN:

```
arr = [10,5,4,3,48,6,2]
k = 4
```

Start

```
Heap = {}
```

Insert 10

```
[10]
```

Insert 5

```
[10,5]
```

Insert 4

```
[10,5,4]
```

Insert 3

```
[10,5,4,3]
```

Heap size = 4

Good.

---

Insert 48

```
[48,10,4,3,5]
```

Size becomes 5

Remove largest

```
Pop 48

Heap

[10,5,4,3]
```

---

Insert 6

```
[10,6,4,3,5]
```

Remove largest

```
Pop 10

Heap

[6,5,4,3]
```

---

Insert 2

```
[6,5,4,3,2]
```

Remove largest

```
Pop 6

Heap

[5,4,3,2]
```

Finished.

```
Top = 5
```

Answer = **5**

---

# IMPORTANT CODE SNIPPETS:

## Max Heap

```cpp
priority_queue<int> pq;
```

or explicitly

```cpp
priority_queue<int, vector<int>, less<int>> pq;
```

### Explanation

`priority_queue` has the syntax:

```cpp
priority_queue<
    DataType,
    Container,
    Comparator
>
```

By default,

```cpp
priority_queue<int>
```

internally means

```cpp
priority_queue<int, vector<int>, less<int>>
```

`less<int>` creates a **Max Heap**, meaning:

```
Largest element stays on top.
```

Example:

```
Heap

10
5
3
2

Top = 10
```

---

## Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Here,

`greater<int>` creates a **Min Heap**.

```
Smallest element stays on top.
```

Example:

```
Heap

2
3
5
10

Top = 2
```

---

## Push

```cpp
pq.push(num);
```

---

## Maintain Heap Size

```cpp
if (pq.size() > k)
    pq.pop();
```

---

## Answer

```cpp
return pq.top();
```

---

# COMMON MISTAKES:

### Mistake 1

Using a Min Heap instead of a Max Heap.

Remember:

- kth Smallest → Max Heap
- kth Largest → Min Heap

---

### Mistake 2

Popping before pushing.

Always

```
Push

↓

If size > k

↓

Pop
```

because the newly inserted element may belong to the smallest `k`.

---

### Mistake 3

Returning after inserting only the first `k` elements.

You must process the **entire array**.

---

### Mistake 4

Thinking the heap is sorted.

It is **not**.

Only the top element is guaranteed.

---

### Mistake 5

Sorting out of habit.

Whenever only **Top K** information is required,

sorting is usually unnecessary.

---

# WHY I MIGHT FORGET THIS:

Both kth Smallest and kth Largest are mirror-image problems.

Easy trick:

```
Need smallest values?

↓

Remove the largest.

↓

Use Max Heap.
```

```
Need largest values?

↓

Remove the smallest.

↓

Use Min Heap.
```

Always remove the element that has become **least useful**.

---

# INTERVIEW FLOW:

**Interviewer:** Find the kth smallest element.

### Step 1

Brute force:

> I can sort the array and return `arr[k-1]` in `O(n log n)` time.

### Step 2

Optimization:

> Since I only need the smallest `k` elements, sorting everything is unnecessary.

### Step 3

Observation:

> I'll maintain only the best `k` candidates using a heap.

### Step 4

Heap Choice:

> Since I need to remove the largest among the current `k` smallest elements whenever a smaller element arrives, I'll use a Max Heap.

### Step 5

Algorithm:

- Push every element.
- If heap size exceeds `k`, remove the largest.
- At the end, the heap contains exactly the `k` smallest elements.
- The top is therefore the kth smallest.

---

# TIME COMPLEXITY:

Let:

- `n` = number of elements
- `k` = required order statistic

### Push Operation

Each insertion into a heap of size at most `k` takes

```
O(log k)
```

because the height of a heap containing `k` elements is

```
log₂(k)
```

---

### Pop Operation

Whenever the heap size exceeds `k`, one element is removed.

Deletion also takes

```
O(log k)
```

---

### Number of Operations

For every element:

- One push
- At most one pop

Each costs `O(log k)`.

So,

```
n × O(log k)

=

O(n log k)
```

### Why not O(n log n)?

The heap **never grows beyond `k` elements**.

The logarithm depends on the heap size, not the array size.

Since

```
k ≤ n
```

and often

```
k << n
```

we get a much faster solution.

---

# SPACE COMPLEXITY:

The heap stores **at most `k` elements**.

No additional data structures are used.

Therefore,

```
O(k)
```

Extra space.

---

# EDGE CASES:

### k = 1

Return the minimum element.

Works correctly.

---

### k = n

Return the maximum element.

Heap grows to size `n`.

Still works.

---

### Duplicate elements

Duplicates are treated as separate elements.

Example:

```
2 2 2 3

k = 2

Answer = 2
```

---

### Single Element

```
n = 1

Answer = arr[0]
```

---

### Already Sorted

Works.

---

### Reverse Sorted

Works.

---

# PATTERN RECOGNITION:

Immediately think of this pattern whenever you hear:

- kth smallest
- kth largest
- Top K elements
- K closest numbers
- K closest points
- K most frequent elements
- Running kth element
- Stream of numbers
- Maintain best K candidates

Ask yourself:

> **Do I only care about the best K elements instead of the entire ordering?**

If YES,

think:

**Fixed-Size Heap**

Then decide:

```
Need smallest K?

↓

Max Heap
```

```
Need largest K?

↓

Min Heap
```

---

# Clean C++ Code

```cpp
class Solution {
public:
    int kthSmallest(vector<int>& arr, int k) {

        priority_queue<int> pq;

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

### Create a Max Heap

```cpp
priority_queue<int> pq;
```

- Stores the current `k` smallest elements.
- Largest among them remains on top.

---

### Traverse every element

```cpp
for (int num : arr)
```

Every element has the potential to be among the smallest `k`.

---

### Insert into heap

```cpp
pq.push(num);
```

Add the current candidate.

---

### Maintain fixed heap size

```cpp
if (pq.size() > k)
    pq.pop();
```

If more than `k` elements exist,

remove the **largest**.

This ensures the heap always stores exactly the smallest `k` elements seen so far.

---

### Return answer

```cpp
return pq.top();
```

The heap now contains the smallest `k` elements.

The largest among them is exactly the kth smallest.

---

# Explain Every Tricky Condition

### Why check `pq.size() > k`?

We first insert because the current element might belong to the smallest `k`.

Only then do we remove the unnecessary element.

This guarantees correctness.

---

### Why return `pq.top()`?

The heap contains exactly the smallest `k` elements.

Among those,

the largest is the kth smallest element in the entire array.

---

# Easy-to-Remember Summary

- **Pattern:** Fixed-Size Top K Heap
- **Heap:** Max Heap
- **Store:** Current `k` smallest elements
- **Push:** Every element
- **Pop:** When heap size exceeds `k`
- **Invariant:** Heap always contains the smallest `k` elements
- **Answer:** `pq.top()`
- **Time:** `O(n log k)`
- **Space:** `O(k)`

### Memory Trick

```
kth Smallest
↓

Keep the smallest k elements

↓

Remove the largest

↓

Max Heap
```

```
kth Largest
↓

Keep the largest k elements

↓

Remove the smallest

↓

Min Heap
```
````
