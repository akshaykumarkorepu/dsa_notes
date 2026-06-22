
## PROBLEM:

Given a stream of integers, after every insertion, return the **Kth largest element** seen so far.

If fewer than **K elements** have been inserted, return **-1**.

> **Important:** This is **Kth largest**, **NOT Kth largest unique**. Duplicate values are counted.

---

# PATTERN:

## Top K Elements (Fixed Size Heap)

This is one of the most common Heap patterns.

### Rule to remember:

> We only care about the **largest K elements**.

So we maintain exactly those K elements throughout the stream.

---

# WHY THIS PATTERN:

The question asks for the **Kth largest after every insertion**.

Think:

> "Do I need all elements?"

No.

To know the Kth largest, we only need the **largest K elements**.

Everything smaller can never become the Kth largest because elements are only inserted, never removed.

So,

- Keep only the largest K elements.
- Throw away everything else.

A Heap allows us to do this efficiently.

---

# CORE IDEA:

## Why Heap?

A Heap gives fast access to one extreme.

Here we want to know:

> Which element should be removed when more than K elements exist?

That should be the **smallest among the largest K**.

That smallest is exactly the **Kth largest**.

Therefore, we use a **Min Heap**.

---

## Why Min Heap?

Suppose

```text
K = 4

Numbers seen:

9 8 7 6
```

Heap contains

```text
6
7
8
9
```

Top = 6

Which is the **4th largest**.

Now suppose 10 comes.

```text
6 7 8 9 10
```

Largest 4 should be

```text
7 8 9 10
```

Who should go?

Smallest → 6

A Min Heap removes it in **O(log K)**.

---

## Why NOT Max Heap?

A Max Heap always removes the largest element.

But we want to **keep** the largest elements.

We want to remove the **smallest** among them.

Hence Min Heap.

---

## What is stored in the Heap?

Only the **largest K elements seen so far.**

---

## Why are elements pushed?

Every incoming element has a chance to belong to the Top K.

So we first insert it.

---

## Why are elements popped?

If Heap size becomes **K+1**,

remove the smallest.

Because it can no longer belong to the largest K elements.

---

## Heap Invariant (Property Maintained)

At every step,

```text
Heap size <= K
```

and

```text
Heap always stores the largest K elements seen so far.
```

Therefore,

```text
Heap top = Kth largest element.
```

---

## Why maintaining only K elements is sufficient?

Suppose

```text
K = 3

Stream:

10 8 7 5 2 1
```

Largest 3 are

```text
10 8 7
```

Will

```text
5
2
1
```

ever become the answer?

No.

They are smaller than the current Kth largest.

So they are useless.

Hence we discard them.

---

# BRUTE FORCE:

## Idea

After every insertion:

- Store all elements seen so far.
- Copy them.
- Sort them in descending order.
- Return the Kth largest.

This works but repeats sorting after every insertion.

---

## Code

```cpp
class Solution {
public:
    vector<int> kthLargest(vector<int>& arr, int k) {

        vector<int> seen;
        vector<int> ans;

        for (int x : arr) {

            // Store current element
            seen.push_back(x);

            // Kth largest doesn't exist yet
            if (seen.size() < k) {
                ans.push_back(-1);
                continue;
            }

            // Copy all elements seen so far
            vector<int> temp = seen;

            // Sort in descending order
            sort(temp.begin(), temp.end(), greater<int>());

            // Kth largest element
            ans.push_back(temp[k - 1]);
        }

        return ans;
    }
};
```

---

## Dry Run

```text
K = 3

Stream:

5
```

Seen

```text
5
```

Less than K

Answer

```text
-1
```

---

Insert

```text
2
```

Seen

```text
5 2
```

Still less than K

```text
-1
```

---

Insert

```text
8
```

Copy

```text
5 2 8
```

Sort descending

```text
8 5 2
```

3rd largest

```text
2
```

---

Insert

```text
6
```

Copy

```text
5 2 8 6
```

Sort

```text
8 6 5 2
```

3rd largest

```text
5
```

---

## Time Complexity

For every insertion,

Sort current stream.

Sorting costs

```text
O(i log i)
```

Overall

```text
O(1log1 + 2log2 + ... + NlogN)

≈ O(N² logN)
```

---

## Space Complexity

Seen array

```text
O(N)
```

Temporary copy

```text
O(N)
```

Overall

```text
O(N)
```

---

# OPTIMAL APPROACH:

Maintain a **Min Heap of size K**.

For every incoming number:

- Push into heap.
- If heap size becomes greater than K, remove the smallest.
- If heap size is smaller than K, answer is -1.
- Otherwise, heap top is the Kth largest.

---

# ALGORITHM:

For every element:

```text
Push into Min Heap.
```

If

```text
Heap size > K
```

```text
Pop top.
```

If

```text
Heap size < K
```

Answer

```text
-1
```

Else

```text
Heap top
```

is the answer.

---

# DRY RUN:

```text
K = 4

Stream

1 2 3 4 5 6
```

---

Insert 1

Heap

```text
1
```

Less than K

Answer

```text
-1
```

---

Insert 2

Heap

```text
1 2
```

Answer

```text
-1
```

---

Insert 3

Heap

```text
1 2 3
```

Answer

```text
-1
```

---

Insert 4

Heap

```text
1 2 3 4
```

Top

```text
1
```

Answer

```text
1
```

---

Insert 5

Push

```text
1 2 3 4 5
```

Too many elements.

Remove smallest

```text
1
```

Heap

```text
2 3 4 5
```

Top

```text
2
```

Answer

```text
2
```

---

Insert 6

Push

```text
2 3 4 5 6
```

Pop

```text
2
```

Heap

```text
3 4 5 6
```

Top

```text
3
```

Answer

```text
3
```

Final Answer

```text
-1 -1 -1 1 2 3
```

---

# IMPORTANT CODE SNIPPETS:

## Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

## Push

```cpp
pq.push(x);
```

---

## Maintain Heap Size K

```cpp
if (pq.size() > k)
    pq.pop();
```

---

## Check if Kth Largest Exists

```cpp
if (pq.size() < k)
    ans.push_back(-1);
else
    ans.push_back(pq.top());
```

---

# COMMON MISTAKES:

### Mistake 1

Using a Max Heap.

Max Heap gives the largest.

We need the **smallest among the largest K**.

Hence Min Heap.

---

### Mistake 2

Keeping every element.

Only the largest K elements matter.

---

### Mistake 3

Returning heap top before heap reaches K elements.

Need

```text
Heap size == K
```

Otherwise answer is -1.

---

### Mistake 4

Thinking duplicates should be removed.

Duplicates count.

---

### Mistake 5

Popping before pushing.

Always

```text
Push

then

Pop if needed.
```

---

# WHY I MIGHT FORGET THIS:

Many students think

> "Largest → Max Heap"

Wrong.

Instead ask:

> **Which element should be removed?**

Need largest K

↓

Remove smallest

↓

**Min Heap**

---

# INTERVIEW FLOW:

> We need the Kth largest after every insertion.

Brute force would sort after every insertion, leading to **O(N² logN)**.

Instead, we only maintain the largest K elements using a Min Heap.

Every incoming element is pushed.

If heap size exceeds K, we remove the smallest because it cannot belong to the largest K anymore.

Thus, the heap always stores exactly the largest K elements.

The smallest among them (heap top) is the Kth largest element.

---

# TIME COMPLEXITY:

## Brute Force

For the i-th insertion,

Sorting costs

```text
O(i log i)
```

Overall

```text
O(N² logN)
```

---

## Optimal

Each insertion:

- Push → O(log K)
- Possible Pop → O(log K)

Overall

```text
O(N log K)
```

### Reason

Heap size never exceeds **K**.

Every heap operation works on at most **K** elements.

---

# SPACE COMPLEXITY:

## Brute Force

```text
O(N)
```

---

## Optimal

Heap stores at most

```text
K
```

elements.

Auxiliary Heap Space

```text
O(K)
```

Including output array

```text
O(N + K)
```

Most platforms mention **O(N)** because the answer array itself contains **N** elements.

---

# EDGE CASES:

- K = 1
  - Every insertion returns the current largest element.

- K = N
  - Only the final insertion produces an answer.

- Duplicate elements
  - Count separately.

- Negative numbers
  - Same logic.

- Already sorted array
  - Works.

- Reverse sorted array
  - Works.

---

# PATTERN RECOGNITION:

Whenever you hear:

- Kth Largest
- Kth Smallest
- Running Kth Largest
- Top K
- Stream of data
- Largest K elements
- Smallest K elements

Think:

> **Fixed Size Heap**

Then ask:

### Which elements do I want to keep?

If keeping

**Largest K**

↓

Use **Min Heap**

If keeping

**Smallest K**

↓

Use **Max Heap**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    vector<int> kthLargest(vector<int>& arr, int k) {

        priority_queue<int, vector<int>, greater<int>> pq;
        vector<int> ans;

        for (int x : arr) {

            // Insert current element
            pq.push(x);

            // Keep only the largest K elements
            if (pq.size() > k)
                pq.pop();

            // Kth largest doesn't exist yet
            if (pq.size() < k)
                ans.push_back(-1);
            else
                ans.push_back(pq.top());
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Create Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

We keep only the **largest K elements**, so the smallest among them should be easy to remove.

---

### Push current element

```cpp
pq.push(x);
```

Every new element might belong to the Top K.

---

### Maintain heap size

```cpp
if (pq.size() > k)
    pq.pop();
```

If more than K elements are present, remove the smallest because it can no longer belong to the largest K.

---

### Check if Kth largest exists

```cpp
if (pq.size() < k)
    ans.push_back(-1);
```

Until K elements are seen, the Kth largest does not exist.

---

### Return answer

```cpp
ans.push_back(pq.top());
```

The smallest element among the largest K elements is exactly the Kth largest.

---

# EXPLAIN EVERY TRICKY CONDITION

### Why `pq.size() > k`?

We first insert the new element.

Only after insertion do we know whether we have exceeded K elements.

Then we remove the smallest.

---

### Why is `pq.top()` the answer?

The heap always contains exactly the largest K elements.

Among them, the smallest is the Kth largest overall.

---

### Why not ignore small incoming elements before pushing?

A new element could still belong to the Top K.

Pushing first and then popping guarantees that the heap always contains the correct K elements without extra comparisons.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Fixed Size Top-K Heap
- **Need:** Kth largest after every insertion
- **Heap:** Min Heap
- **Store:** Largest K elements only
- **Push:** Every incoming element
- **Pop:** If heap size exceeds K
- **Invariant:** Heap always contains the largest K elements
- **Answer:** Heap top
- **Time:** O(N log K)

### Heap Rule to Remember

- **Largest K → Min Heap**
- **Smallest K → Max Heap**
````
