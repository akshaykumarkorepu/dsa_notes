
# Top K Frequent Elements in an Array

---

# PROBLEM:

Given an integer array `arr[]` and an integer `k`, return the **k elements having the highest frequency**.

**Tie-breaking Rule:** If two elements have the same frequency, the **larger element gets higher priority**.

### Example

```text
Input:
arr = [3,1,4,4,5,2,6,1]
k = 2

Frequency:
1 → 2
4 → 2
others → 1

Output:
[4,1]
```

Since both `1` and `4` have the same frequency, `4` comes first because it is larger.

---

# PATTERN:

**Top K Elements using Heap**

This is one of the most common Heap interview patterns.

Whenever you see:

- Top K Frequent
- K Largest
- K Smallest
- K Closest
- K Most Frequent

Start thinking about a **Heap**.

---

# WHY THIS PATTERN:

A full sorting approach sorts every unique element.

However, we only need the **best K elements**.

Instead of maintaining every element in sorted order, we maintain only the current **Top K candidates** using a heap.

This avoids unnecessary sorting.

---

# CORE IDEA:

There are two observations.

### Observation 1

Frequency is what matters.

First count frequencies using a HashMap.

```
5 → 3
2 → 1
7 → 2
11 → 2
```

Now the problem becomes:

> Find the K pairs having the maximum frequency.

---

### Observation 2

If frequencies are equal,

the **larger value wins**.

Ordering becomes:

```
Higher frequency first

If frequencies are equal

Higher value first
```

---

# BRUTE FORCE:

## Idea

1. Count frequencies.
2. Store every unique element as

```
(frequency, value)
```

3. Sort using:

- Higher frequency first
- If frequencies are equal, larger value first

4. Take the first `k` elements.

---

## Code

```cpp
class Solution {
public:
    vector<int> topKFreq(vector<int> &arr, int k) {

        unordered_map<int,int> freq;

        for(int x : arr){
            freq[x]++;
        }

        vector<pair<int,int>> v;

        for(auto &p : freq){
            v.push_back({p.second,p.first});
        }

        sort(v.begin(), v.end(), [](auto &a, auto &b){

            if(a.first == b.first)
                return a.second > b.second;

            return a.first > b.first;
        });

        vector<int> ans;

        for(int i=0;i<k;i++)
            ans.push_back(v[i].second);

        return ans;
    }
};
```

---

## Dry Run

```
arr

[7,10,11,5,2,5,5,7,11,8,9]
```

Frequency

```
5 → 3
11 → 2
7 → 2
10 → 1
2 → 1
8 → 1
9 → 1
```

Store

```
(3,5)
(2,11)
(2,7)
(1,10)
(1,2)
(1,8)
(1,9)
```

Sort

```
(3,5)

(2,11)

(2,7)

(1,10)

(1,9)

(1,8)

(1,2)
```

Take first four

```
5
11
7
10
```

Answer

```
[5,11,7,10]
```

---

## Time Complexity

Let

- `n = size of array`
- `m = number of distinct elements`

### Frequency counting

Traverse the array once.

```
O(n)
```

### Store unique elements

Traverse the hash map once.

```
O(m)
```

### Sorting

Sort `m` pairs.

```
O(m log m)
```

### Extract first k elements

```
O(k)
```

### Overall

```
O(n + m log m + k)
```

Since

```
k ≤ m ≤ n
```

Worst case

```
O(n log n)
```

---

## Space Complexity

Frequency Map

```
O(m)
```

Vector of pairs

```
O(m)
```

Answer

```
O(k)
```

Total

```
O(m + k)
```

Worst case

```
O(n)
```

---

# OPTIMAL APPROACH:

Instead of sorting all unique elements,

keep only the **Top K** inside a Heap.

---

## Why Heap?

Suppose

```
100000 unique numbers

k = 10
```

Sorting all 100000 elements is unnecessary.

We only care about the best 10.

A heap continuously removes bad candidates.

---

## Which Heap?

### Min Heap

This is a **Top K** problem.

Whenever we need to maintain only the best `k` elements,

use a **Min Heap**.

---

## Why Min Heap?

The smallest (worst) candidate among the current Top K stays at the top.

Whenever a better candidate arrives,

remove the weakest one.

---

## What is stored in the Heap?

Each heap node stores

```
(frequency, value)
```

Example

```
(3,5)

means

frequency = 3
value = 5
```

---

## Why are elements pushed?

Every unique element is a possible answer.

So every `(frequency, value)` pair is inserted.

---

## Why are elements popped?

Whenever heap size becomes

```
k + 1
```

remove the weakest candidate.

---

## Heap Invariant

At every moment,

the heap contains **only the best K elements seen so far**.

The top of the heap is always

```
the weakest among those K elements.
```

---

## Why maintaining only K elements is sufficient?

Suppose

```
k = 3
```

After processing many unique elements,

we never need the weaker ones.

Once an element is worse than the current Top 3,

it can never become part of the answer.

So we discard it immediately.

---

# ALGORITHM:

### Step 1

Count frequencies.

```
unordered_map
```

---

### Step 2

Create a Min Heap storing

```
(frequency, value)
```

---

### Step 3

For every unique element

```
push into heap
```

---

### Step 4

If heap size exceeds `k`

```
pop
```

---

### Step 5

Heap now contains the Top K elements.

Extract them.

---

### Step 6

Reverse the answer because Min Heap removes the smallest first.

---

# DRY RUN

Example

```
arr

[7,10,11,5,2,5,5,7,11,8,9]

k = 4
```

Frequency

```
5 → 3
11 → 2
7 → 2
10 → 1
2 → 1
8 → 1
9 → 1
```

Heap evolution

Push

```
(3,5)
```

Push

```
(2,11)
```

Push

```
(2,7)
```

Push

```
(1,10)
```

Heap size = 4

Keep all.

Push

```
(1,2)
```

Heap size = 5

Pop

```
(1,2)
```

Push

```
(1,8)
```

Pop

```
(1,8)
```

Push

```
(1,9)
```

Pop

```
(1,9)
```

Remaining

```
(1,10)

(2,7)

(2,11)

(3,5)
```

Extract

```
10

7

11

5
```

Reverse

```
5

11

7

10
```

Final Answer

```
[5,11,7,10]
```

---

# IMPORTANT CODE SNIPPETS

### Frequency Map

```cpp
unordered_map<int,int> freq;

for(int x : arr)
    freq[x]++;
```

Counts frequency of every element.

---

### Min Heap

```cpp
priority_queue<
pair<int,int>,
vector<pair<int,int>>,
greater<pair<int,int>>
> pq;
```

Stores

```
(frequency, value)
```

Smallest frequency stays at the top.

---

### Push

```cpp
pq.push({p.second,p.first});
```

Store

```
(frequency, value)
```

---

### Maintain only Top K

```cpp
if(pq.size() > k)
    pq.pop();
```

Remove the weakest candidate.

---

### Reverse

```cpp
reverse(ans.begin(), ans.end());
```

Min Heap returns smallest first.

Reverse gives highest priority first.

---

# COMMON MISTAKES

### Mistake 1

Storing

```
(value, frequency)
```

instead of

```
(frequency, value)
```

Heap ordering becomes incorrect.

---

### Mistake 2

Forgetting to reverse.

---

### Mistake 3

Thinking Top K means Max Heap.

For maintaining only K elements,

Top K almost always uses a **Min Heap**.

---

### Mistake 4

Ignoring tie-breaking.

When frequencies are equal,

larger value should come first.

---

### Important Note about the Given Optimal Code

The heap uses

```cpp
greater<pair<int,int>>
```

Pairs are compared lexicographically.

```
(frequency, value)
```

So

- Smaller frequency comes first.
- If frequencies are equal,
  smaller value comes first.

Since the heap removes the smallest pair,

the **smaller value gets removed first**,

leaving the **larger value** inside the heap.

Thus the tie-breaking rule is automatically satisfied.

---

# WHY I MIGHT FORGET THIS

People usually think

```
Top K

↓

Max Heap
```

Actually,

Top K usually means

```
Fixed Size Min Heap
```

because we keep removing the weakest candidate.

---

# INTERVIEW FLOW

> "I'll first count the frequency of every element using a HashMap.

Each unique element becomes a `(frequency, value)` pair.

A brute-force solution is to sort all pairs by descending frequency and descending value.

However, sorting every unique element is unnecessary because only the best `k` are required.

So I'll maintain a Min Heap of size `k`.

Each pair is inserted into the heap.

Whenever the heap size exceeds `k`, I remove the weakest candidate.

At the end, the heap contains exactly the Top K frequent elements.

Finally, I extract them and reverse the answer because a Min Heap returns the smallest element first."

---

# TIME COMPLEXITY

Let

- `n = size of array`
- `m = number of distinct elements`

### Frequency counting

Traverse the array once.

```
O(n)
```

---

### Heap Insertions

There are `m` unique elements.

Each insertion into a heap of maximum size `k` takes

```
O(log k)
```

Total

```
O(m log k)
```

---

### Heap Removals

Whenever heap size exceeds `k`,

one pop is performed.

Each pop costs

```
O(log k)
```

Overall heap operations remain

```
O(m log k)
```

---

### Extract Answer

Heap contains exactly `k` elements.

Removing all of them takes

```
k × O(log k)

= O(k log k)
```

Reverse

```
O(k)
```

---

### Overall

```
O(n + m log k + k log k)
```

Since

```
k ≤ m
```

it is commonly written as

```
O(n + m log k)
```

Worst case

```
O(n log k)
```

This is better than

```
O(n log n)
```

when `k` is much smaller than `n`.

---

# SPACE COMPLEXITY

### Frequency Map

Stores one entry for each distinct element.

```
O(m)
```

---

### Heap

Maximum size

```
k
```

Space

```
O(k)
```

---

### Answer

Stores

```
k
```

elements.

```
O(k)
```

---

### Total

```
O(m + k)
```

Since

```
k ≤ m
```

it is commonly simplified to

```
O(m)
```

Worst case

```
O(n)
```

---

# EDGE CASES

- Only one unique element.
- `k = 1`
- `k = number of distinct elements`
- All elements identical.
- Every element appears once.
- All frequencies equal (larger values come first).

---

# PATTERN RECOGNITION

Use this pattern whenever you see

- Top K Frequent Elements
- K Largest Elements
- K Smallest Elements
- K Closest Points
- K Closest Numbers
- K Most Frequent Words
- K Largest Sum Combinations

### Rule of Thumb

Need all elements sorted?

→ Sort.

Need only the best K?

→ **Fixed Size Min Heap.**

Need repeated largest/smallest?

→ Max Heap / Min Heap depending on the problem.

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<int> topKFreq(vector<int> &arr, int k) {

        unordered_map<int,int> freq;

        for(int x : arr){
            freq[x]++;
        }

        priority_queue<
            pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>
        > pq;

        for(auto &p : freq){

            pq.push({p.second, p.first});

            if(pq.size() > k){
                pq.pop();
            }
        }

        vector<int> ans;

        while(!pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }

        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
unordered_map<int,int> freq;
```

Stores the frequency of every number.

---

```cpp
freq[x]++;
```

Counts how many times each element appears.

---

```cpp
priority_queue<
pair<int,int>,
vector<pair<int,int>>,
greater<pair<int,int>>
> pq;
```

Creates a Min Heap ordered by

```
(frequency, value)
```

---

```cpp
pq.push({p.second, p.first});
```

Pushes

```
(frequency, value)
```

into the heap.

---

```cpp
if(pq.size() > k)
    pq.pop();
```

Maintains only the best `k` candidates.

---

```cpp
ans.push_back(pq.top().second);
```

Stores only the value.

---

```cpp
reverse(ans.begin(), ans.end());
```

Min Heap returns weakest first.

Reverse gives highest priority first.

---

# Explain Every Tricky Condition

### Why `pq.size() > k` instead of `>= k`?

We first insert the current candidate.

If the heap now has `k + 1` elements,

we remove the weakest one.

This ensures every candidate gets considered.

---

### Why does `greater<pair<int,int>>` handle tie-breaking?

Pairs are compared lexicographically.

```
(frequency, value)
```

Comparison order

1. Smaller frequency
2. Smaller value

The heap removes the smallest pair.

So among equal frequencies,

the **smaller value gets removed first**,

leaving the **larger value** inside the heap,

which matches the problem statement.

---

# Easy-to-Remember Summary

- Count frequencies.
- Convert every unique number into `(frequency, value)`.
- Use a **Min Heap of size K**.
- Push every pair.
- If heap size exceeds `k`, pop.
- Extract all remaining elements.
- Reverse the answer.
- **Top K ⇒ Fixed Size Min Heap.**
````
