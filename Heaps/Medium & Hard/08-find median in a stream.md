
# Find Median in a Stream

## PROBLEM:

Given a stream of integers, after every insertion return the **median of all elements seen so far**.

- **Odd number of elements:** Median is the middle element.
- **Even number of elements:** Median is the average of the two middle elements.

Example:

```
Stream:
5
5 15
5 15 1
5 15 1 3
...

Median:
5
10
5
4
...
```

---

# PATTERN:

**Two Heaps (Max Heap + Min Heap)**

Whenever you continuously insert elements and repeatedly need the **median**, think:

> **Two Heaps**

---

# WHY THIS PATTERN:

Ask yourself:

> **What information is actually needed to find the median?**

The median only depends on the **middle of the sorted order**.

We do **not** need the entire array sorted after every insertion.

Instead, we only need:

- Largest element of the smaller half
- Smallest element of the larger half

A heap gives these in **O(1)** time.

So we split the numbers into two halves.

```
Smaller Half        Larger Half

1 2 3 | 5 7 8 10

      ^
   Median lies here
```

We maintain:

- Left half → Max Heap
- Right half → Min Heap

Now the median is always available from the heap tops.

---

# CORE IDEA:

Maintain two heaps.

## Left Heap (Max Heap)

Stores the **smaller half**.

```
Top = Largest element of smaller half
```

Example:

```
1 2 3

Top = 3
```

---

## Right Heap (Min Heap)

Stores the **larger half**.

```
Top = Smallest element of larger half
```

Example:

```
5 7 8 10

Top = 5
```

---

Median becomes:

Odd:

```
Left Top
```

Even:

```
(Left Top + Right Top) / 2
```

---

### Why Max Heap on Left?

We need the **largest element of the smaller half**.

Max Heap gives it instantly.

---

### Why Min Heap on Right?

We need the **smallest element of the larger half**.

Min Heap gives it instantly.

---

# BRUTE FORCE:

## Idea

After every insertion:

```
Insert into vector

Sort vector

Find median
```

### Code

```cpp
class Solution {
public:
    vector<double> getMedian(vector<int> &arr) {

        vector<int> nums;
        vector<double> ans;

        for (int x : arr) {

            nums.push_back(x);

            sort(nums.begin(), nums.end());

            int n = nums.size();

            if (n % 2 == 1)
                ans.push_back(nums[n / 2]);
            else
                ans.push_back((nums[n/2] + nums[n/2-1]) / 2.0);
        }

        return ans;
    }
};
```

---

### Dry Run

Stream:

```
5

nums = [5]

Median = 5
```

Insert 15

```
nums = [5,15]

Sort

[5,15]

Median = (5+15)/2 = 10
```

Insert 1

```
nums = [5,15,1]

Sort

[1,5,15]

Median = 5
```

---

### Time Complexity

For every insertion we sort the current array.

Sorting sizes become:

```
1
2
3
...
n
```

Total work:

```
1log1
+
2log2
+
3log3
...
+
nlogn
```

Overall:

```
O(n² log n)
```

---

### Space Complexity

Store all elements.

```
O(n)
```

---

# OPTIMAL APPROACH:

Use **Two Heaps**.

```
Left = Max Heap

Right = Min Heap
```

---

## What is stored?

Left

```
Smaller Half
```

Right

```
Larger Half
```

---

## Why Heap is the Correct Data Structure?

The median only depends on the **boundary between two halves**.

Heap gives:

- Fast insertion → O(log n)
- Fast access to boundary element → O(1)

Balanced BST also works, but Heap is simpler and more efficient for this problem.

---

## Why elements are pushed?

Every new number must belong to either:

- Smaller half
- Larger half

So every incoming element is pushed into one of the heaps.

---

## Why elements are popped?

Insertion may make one heap larger than allowed.

Example:

```
Left

1 2 3 4 5

Right

10
```

Left has too many elements.

Move one element to Right.

---

# HEAP INVARIANTS (MOST IMPORTANT)

Everything works because these two invariants are maintained after every insertion.

---

## Invariant 1 — Size Property

```
left.size() == right.size()

OR

left.size() == right.size() + 1
```

Meaning:

Left either has:

```
Equal elements
```

or

```
Exactly one extra element
```

Never:

```
Left bigger by 2

or

Right bigger than Left
```

---

### Why?

For an odd number of elements:

```
Median is one element.

We choose Left to hold it.
```

Example:

```
1 2 3

Left

1 2

Right

3
```

Left has one extra.

Median = Left Top.

---

Even case:

```
1 2 3 4

Left

1 2

Right

3 4
```

Equal sizes.

Median = Average.

Whenever this property breaks:

Move one element between heaps.

---

## Invariant 2 — Ordering Property

Every element in Left must be:

```
<=
```

Every element in Right.

Example:

```
Left

1 2 4

Right

5 8 9
```

Equivalent condition:

```
left.top() <= right.top()
```

(when both heaps are non-empty)

---

### Why is this necessary?

Suppose:

```
Left

1 7

Right

5 9
```

Now:

```
7 > 5
```

The halves are mixed.

Sorted order is actually:

```
1 5 7 9
```

Median is no longer represented by heap tops.

Correct arrangement:

```
Left

1 5

Right

7 9
```

Now the heaps correctly represent the two halves.

---

### How is this maintained?

Insertion rule:

```
if(x <= left.top())

    Left

else

    Right
```

Then rebalance.

Only heap tops move during rebalancing, so ordering remains correct.

---

### Why does this guarantee Left Top is the Median?

Sorted array:

```
1 2 3 5 7 8
```

Split:

```
Left

1 2 3

Right

5 7 8
```

Largest in Left:

```
3
```

Smallest in Right:

```
5
```

These are exactly the two middle numbers.

Median:

```
(3+5)/2
```

Odd case:

```
1 2 3 5 7
```

Split:

```
Left

1 2 3

Right

5 7
```

Largest in Left:

```
3
```

This is exactly the median.

---

# ALGORITHM:

For every incoming number:

### Step 1

Insert.

```
if(left empty OR x <= left.top())

    push into Left

else

    push into Right
```

---

### Step 2

Rebalance.

If:

```
Left bigger by 2
```

Move Left Top → Right.

If:

```
Right bigger
```

Move Right Top → Left.

---

### Step 3

Find median.

If sizes equal:

```
(left.top()+right.top())/2
```

Else:

```
left.top()
```

Repeat.

---

# DRY RUN:

Input:

```
5 15 1 3 2 8
```

---

### Insert 5

```
Left

5

Right

-
```

Median:

```
5
```

---

### Insert 15

```
Left

5

Right

15
```

Median:

```
(5+15)/2 = 10
```

---

### Insert 1

```
Left

5 1

Right

15
```

Median:

```
5
```

---

### Insert 3

Initially:

```
Left

5 3 1

Right

15
```

Too many in Left.

Move 5.

```
Left

3 1

Right

5 15
```

Median:

```
(3+5)/2 = 4
```

---

### Insert 2

```
Left

3 2 1

Right

5 15
```

Median:

```
3
```

---

### Insert 8

```
Left

3 2 1

Right

5 8 15
```

Median:

```
(3+5)/2 = 4
```

---

# IMPORTANT CODE SNIPPETS:

## Max Heap

```cpp
priority_queue<int> left;
```

---

## Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> right;
```

---

## Insert

```cpp
if(left.empty() || x <= left.top())
    left.push(x);
else
    right.push(x);
```

---

## Rebalance

```cpp
if(left.size() > right.size()+1){
    right.push(left.top());
    left.pop();
}
else if(right.size() > left.size()){
    left.push(right.top());
    right.pop();
}
```

---

## Median

```cpp
if(left.size()==right.size())
    median = (left.top()+right.top())/2.0;
else
    median = left.top();
```

---

# COMMON MISTAKES:

- Forgetting to rebalance heaps.
- Using Min Heap on Left.
- Using integer division instead of `/2.0`.
- Returning `right.top()` when total elements are odd.
- Forgetting `left.empty()` check before accessing `left.top()`.

---

# WHY I MIGHT FORGET THIS:

People usually think:

> Median requires sorting.

Actually,

Median only depends on the **middle boundary**, not the full sorted order.

Remember only two rules:

1. Left stores the smaller half, Right stores the larger half.
2. Left always has either the same number of elements as Right or exactly one more.

If these are maintained, the median is always available at the heap tops.

---

# INTERVIEW FLOW:

### Step 1

Clarify the problem.

> We need the median after every insertion.

---

### Step 2

Explain brute force.

> Store all numbers, sort after every insertion, then compute the median.

Time Complexity:

```
O(n² log n)
```

Too slow.

---

### Step 3

Optimization

Observe that:

> We don't need the entire array sorted.

We only need the boundary between the two halves.

---

### Step 4

Choose data structures.

- Max Heap → Smaller half
- Min Heap → Larger half

---

### Step 5

Explain the invariants.

**Ordering Invariant**

```
Every Left element <= Every Right element
```

**Size Invariant**

```
Left size == Right size

OR

Left size == Right size + 1
```

These two invariants guarantee that:

- Odd → Left Top is the median.
- Even → Average of both tops is the median.

---

### Step 6

Explain insertion.

- Compare with `left.top()`.
- Insert into correct heap.
- Rebalance if necessary.

---

### Step 7

Explain median calculation.

Equal sizes:

```
Average
```

Else:

```
Left Top
```

---

### Step 8

Mention complexity.

Each insertion:

- Heap insertion → O(log n)
- Rebalance → O(log n)
- Median retrieval → O(1)

Overall:

```
O(n log n)
```

---

# TIME COMPLEXITY:

## Per Insertion

### Heap Insertion

Each heap may contain up to **n** elements.

Insertion into a heap takes:

```
O(log n)
```

because the new element may move upward through the heap.

---

### Rebalancing

At most one element moves between heaps.

One move consists of:

- One Pop → O(log n)
- One Push → O(log n)

Overall:

```
O(log n)
```

---

### Finding Median

Heap Top is accessed directly.

```
O(1)
```

---

### Total Per Insertion

```
O(log n)
```

---

### Total for n Insertions

```
n × O(log n)

=

O(n log n)
```

---

# SPACE COMPLEXITY:

Both heaps together store every element.

After processing all n elements:

```
Left Heap + Right Heap

=

n elements
```

Therefore:

```
O(n)
```

---

# EDGE CASES:

- Single element.
- Two elements.
- All elements equal.
- Strictly increasing order.
- Strictly decreasing order.
- Duplicate values.
- Large values (always divide by `2.0`).

---

# PATTERN RECOGNITION:

Whenever you hear:

- Running Median
- Median in Stream
- Online Median
- Dynamic Median
- Median after every insertion

Immediately think:

> **Two Heaps**

Ask yourself:

1. Can I divide the data into two halves?
2. Do I only need the boundary between those halves?
3. Can Max Heap maintain the lower boundary?
4. Can Min Heap maintain the upper boundary?

If yes,

**Two Heaps is the correct pattern.**

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<double> getMedian(vector<int> &arr) {

        // Max Heap -> Smaller Half
        priority_queue<int> left;

        // Min Heap -> Larger Half
        priority_queue<int, vector<int>, greater<int>> right;

        vector<double> ans;

        for (int x : arr) {

            // Insert into appropriate heap
            if (left.empty() || x <= left.top()) {
                left.push(x);
            } else {
                right.push(x);
            }

            // Rebalance if Left has more than one extra element
            if (left.size() > right.size() + 1) {
                right.push(left.top());
                left.pop();
            }

            // Rebalance if Right becomes larger
            else if (right.size() > left.size()) {
                left.push(right.top());
                right.pop();
            }

            // Compute Median
            if (left.size() == right.size()) {
                ans.push_back((left.top() + right.top()) / 2.0);
            } else {
                ans.push_back(left.top());
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
priority_queue<int> left;
```

- Max Heap.
- Stores the smaller half.
- Top is the largest element of the smaller half.

---

```cpp
priority_queue<int, vector<int>, greater<int>> right;
```

- Min Heap.
- Stores the larger half.
- Top is the smallest element of the larger half.

---

```cpp
if(left.empty() || x <= left.top())
```

If the incoming number belongs to the smaller half, place it in Left.

---

```cpp
left.size() > right.size() + 1
```

Left is allowed to have **only one extra element**.

Otherwise rebalance.

---

```cpp
right.size() > left.size()
```

Right should never contain more elements than Left.

Move one element back.

---

```cpp
(left.top() + right.top()) / 2.0
```

Using `2.0` ensures floating-point division.

---

# Explain Every Tricky Condition

### `x <= left.top()`

Maintains the ordering invariant.

Numbers smaller than the current boundary belong to the left half.

---

### `left.size() > right.size() + 1`

Maintains the balance invariant.

Left cannot have more than one extra element.

---

### `right.size() > left.size()`

Left is always chosen to hold the extra element.

If Right becomes larger, move one element back.

---

### `left.size() == right.size()`

Even number of elements.

Median is the average of the two middle elements.

---

# Easy-to-Remember Summary

- Pattern → **Two Heaps**
- Left = **Max Heap** (Smaller Half)
- Right = **Min Heap** (Larger Half)
- Ordering Invariant → Every Left element ≤ Every Right element.
- Size Invariant → Left has equal size or exactly one extra element.
- Insert → Compare with `left.top()`, then rebalance.
- Median:
  - Equal sizes → Average of both tops.
  - Otherwise → `left.top()`.
- Time Complexity → **O(n log n)**
- Space Complexity → **O(n)**
````
