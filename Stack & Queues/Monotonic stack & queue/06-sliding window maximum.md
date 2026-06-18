
# K Sized Subarray Maximum



## PROBLEM

Given an array `arr[]` and an integer `k`, find the **maximum element in every contiguous subarray (window) of size `k`**.

### Example

```text
arr = [1,2,3,1,4,5,2,3,6]
k = 3

Windows

[1 2 3] → 3
[2 3 1] → 3
[3 1 4] → 4
[1 4 5] → 5
[4 5 2] → 5
[5 2 3] → 5
[2 3 6] → 6

Answer

[3,3,4,5,5,5,6]
```

---

## PATTERN

**Sliding Window + Monotonic Deque (Monotonically Decreasing Queue)**

---

## WHY THIS PATTERN

This problem asks us to repeatedly process **contiguous windows of fixed size**.

Whenever you see:

- Contiguous subarray
- Window of fixed size `k`
- Maximum/Minimum in every window

Think:

> **Sliding Window**

However, scanning every window completely is inefficient.

Instead, we maintain only the **useful candidates** for the maximum using a **Monotonic Deque**, allowing us to answer each window efficiently.

---

## CORE IDEA

The **deque does NOT store the entire window.**

This is the biggest misconception.

Instead, it stores **only the indices of elements that can still become the maximum**.

The algorithm follows these rules:

- When a larger element arrives, all smaller elements behind it become useless and are removed.
- When an element leaves the current window, it is removed from the front.
- Therefore, the **front of the deque is always the maximum element of the current window.**

---

## WHAT IS STORED IN THE DEQUE?

The deque stores **indices**, not values.

```text
Array

Index : 0 1 2 3 4
Value : 5 2 4 1 7
```

Deque:

```text
0 2
```

represents

```text
Values

5 4
```

### Why store indices instead of values?

Because we must answer:

> **"Has this element gone outside the current window?"**

Only indices allow us to check this.

---

## WHY DO WE USE A DEQUE?

We need four operations efficiently:

```text
Remove expired element
↓
pop_front()
```

```text
Remove smaller elements
↓
pop_back()
```

```text
Insert current element
↓
push_back()
```

```text
Get current maximum
↓
front()
```

All of these operations are **O(1)** in a deque.

---

## WHAT PROPERTY DOES THE DEQUE MAINTAIN?

The deque is always maintained in **Monotonically Decreasing Order**.

Example:

```text
9 7 5 2
```

The largest element is always at the front.

Therefore,

```text
Front = Maximum
```

---

## WHY ARE ELEMENTS PUSHED?

Every new element enters the sliding window.

It might become:

- The current maximum
- A future maximum

So we insert it into the deque.

---

## WHY ARE ELEMENTS POPPED FROM THE FRONT?

They have **left the current window**, so they can never contribute to future answers.

---

## WHY ARE ELEMENTS POPPED FROM THE BACK?

Example:

```text
Deque

5 2
```

Incoming element:

```text
4
```

Question:

Can `2` ever become the maximum while `4` is still in the window?

No.

Since `4` is larger and arrived later, `2` will never be useful again.

Therefore, remove it.

---

## MONOTONIC PROPERTY

The deque is always maintained in **decreasing order**.

Example:

```text
8 6 4 2
```

Incoming:

```text
9
```

All smaller elements are removed.

Deque becomes:

```text
9
```

This guarantees that the front always stores the maximum.

---

## WHY IS THE DATA STRUCTURE NECESSARY?

Without the deque:

- Every window scans all `k` elements.
- Time Complexity becomes `O(nk)`.

The deque remembers only the useful candidates, allowing us to find the maximum of every window in **O(1)** after processing the new element.

---

# BRUTE FORCE

## Intuition

For every window:

- Scan all `k` elements.
- Find the maximum.
- Store the answer.

---

### Example

```text
arr = [1,2,3,1,4]
k = 3
```

Window 1

```text
1 2 3
```

Maximum = 3

---

Window 2

```text
2 3 1
```

Maximum = 3

---

Window 3

```text
3 1 4
```

Maximum = 4

Answer:

```text
3 3 4
```

---

## Brute Force Code

```cpp
vector<int> maxOfSubarrays(vector<int>& arr, int k) {

    int n = arr.size();
    vector<int> ans;

    for(int i = 0; i <= n-k; i++) {

        int maxi = arr[i];

        for(int j = i; j < i+k; j++) {
            maxi = max(maxi, arr[j]);
        }

        ans.push_back(maxi);
    }

    return ans;
}
```

---

## Brute Force Dry Run

```text
arr = [1,2,3,1,4]
k = 3
```

### Window 1

```text
1 2 3
```

Scan:

```text
1 → 2 → 3
```

Maximum:

```text
3
```

---

### Window 2

```text
2 3 1
```

Scan:

```text
2 → 3 → 1
```

Maximum:

```text
3
```

---

### Window 3

```text
3 1 4
```

Scan:

```text
3 → 1 → 4
```

Maximum:

```text
4
```

Answer:

```text
3 3 4
```

---

## TIME COMPLEXITY

Outer loop:

```text
n-k+1 windows
```

Inner loop:

```text
k elements
```

Overall:

```text
O((n-k+1) × k)

≈ O(nk)
```

---

## SPACE COMPLEXITY

Only one extra variable (`maxi`) is used.

Auxiliary Space:

```text
O(1)
```

(Output array is excluded.)

---

## WHY DO WE NEED TO OPTIMIZE?

Notice:

Window 1

```text
1 2 3
```

Window 2

```text
2 3 1
```

Elements `2` and `3` were already scanned.

Yet we scan them again.

This repeated work leads to `O(nk)` complexity.

We need a way to:

- Avoid rescanning old elements.
- Instantly know the maximum of the current window.

This naturally leads to the **Sliding Window + Monotonic Deque** approach.

---

# OPTIMAL APPROACH

## Intuition

The brute force repeatedly scans every window.

Example:

```text
Window 1

5 2 4
```

↓

```text
Window 2

2 4 1
```

Notice that:

```text
2
4
```

were already scanned.

Yet we scan them again.

This repeated work leads to:

```text
O(nk)
```

Instead of storing every element of the window, we should keep only the elements that can still become the maximum.

---

## Key Observation

Suppose the current window is:

```text
5 2 4
```

Question:

Can

```text
2
```

ever become the maximum?

**No.**

Why?

Because:

- `4` is larger.
- `4` entered later, so it will stay in the window longer.

Even after:

```text
5
```

leaves the window,

the window becomes

```text
2 4
```

and the maximum is still

```text
4
```

Therefore,

```text
2
```

can never become the maximum again.

So we remove it.

This is the core intuition behind the **Monotonic Deque**.

---

# OPTIMAL APPROACH

Maintain a **Monotonically Decreasing Deque** that stores **indices**.

For every new element:

1. Remove indices that are outside the current window.
2. Remove all smaller elements from the back.
3. Insert the current index.
4. Once the first complete window is formed, the front of the deque is the maximum.

---

# ALGORITHM

For every index `i`:

### Step 1: Remove Expired Indices

```cpp
while(!dq.empty() && dq.front() <= i-k)
    dq.pop_front();
```

Removes indices that have already gone outside the current window.

---

### Step 2: Remove Smaller Elements

```cpp
while(!dq.empty() && arr[dq.back()] <= arr[i])
    dq.pop_back();
```

Removes elements that can never become the maximum because a larger element has arrived.

---

### Step 3: Insert Current Index

```cpp
dq.push_back(i);
```

The current element is now a new candidate for the maximum.

---

### Step 4: Store the Answer

```cpp
if(i >= k-1)
    ans.push_back(arr[dq.front()]);
```

Once the first complete window is formed, the front of the deque always contains the maximum.

---

# COMPLETE CODE

```cpp
class Solution {
public:
    vector<int> maxOfSubarrays(vector<int>& arr, int k) {

        int n = arr.size();

        vector<int> ans;
        deque<int> dq;

        for(int i = 0; i < n; i++) {

            while(!dq.empty() && dq.front() <= i-k)
                dq.pop_front();

            while(!dq.empty() && arr[dq.back()] <= arr[i])
                dq.pop_back();

            dq.push_back(i);

            if(i >= k-1)
                ans.push_back(arr[dq.front()]);
        }

        return ans;
    }
};
```

---

# ENGLISH TRANSLATION OF THE CODE

## Line 1

```cpp
vector<int> ans;
```

**English:**

> Store the maximum of every window in this vector.

---

## Line 2

```cpp
deque<int> dq;
```

**English:**

> Maintain a deque containing the indices of useful candidates for the maximum.

---

## Line 3

```cpp
for(int i=0;i<n;i++)
```

**English:**

> Process every element exactly once.
>
> Think of each iteration as **one new element entering the sliding window**.

> **Important:** The loop iterates over **elements**, **not windows**.

---

## Line 4

```cpp
while(!dq.empty() && dq.front() <= i-k)
    dq.pop_front();
```

**English:**

> Remove every index that is no longer inside the current window.

### Why `i-k`?

Suppose:

```text
k = 3
i = 5
```

Current window:

```text
Indices

3 4 5
```

Indices outside the window:

```text
0
1
2
```

Notice:

```text
i-k = 5-3 = 2
```

Therefore,

everything with index

```text
<= 2
```

must be removed.

---

## Line 5

```cpp
while(!dq.empty() && arr[dq.back()] <= arr[i])
    dq.pop_back();
```

**English:**

> Remove every smaller element because the current larger element is a better candidate for future maximums.

Example:

Current deque:

```text
5 2
```

Incoming element:

```text
4
```

Can `2` ever become the maximum?

No.

Remove it.

Deque:

```text
5
```

Push `4`:

```text
5 4
```

---

## Line 6

```cpp
dq.push_back(i);
```

**English:**

> The current element has entered the window.
>
> Add it as a possible future maximum.

### Why after the while loops?

Suppose:

```text
Deque

9 7 5
```

Incoming:

```text
8
```

If we push first:

```text
9 7 5 8
```

The deque is no longer decreasing.

Wrong.

Instead:

```text
9 7 5

↓

Remove 5

↓

Remove 7

↓

9

↓

Push 8

↓

9 8
```

The decreasing order is preserved.

---

## Line 7

```cpp
if(i >= k-1)
```

**English:**

> Have I processed enough elements to form the first complete window?

Example:

```text
k = 3
```

Iteration:

```text
i = 0
```

Seen:

```text
1 element
```

No answer.

---

Iteration:

```text
i = 1
```

Seen:

```text
2 elements
```

No answer.

---

Iteration:

```text
i = 2
```

Seen:

```text
3 elements
```

The first complete window is formed.

Now produce the first answer.

---

## Line 8

```cpp
ans.push_back(arr[dq.front()]);
```

**English:**

> The front of the deque is always the maximum element of the current window.
>
> Store it in the answer.

---

# COMPLETE DRY RUN

```text
Array

5 2 4 1 7

k = 3
```

---

## Iteration 0

Current element:

```text
5
```

Remove expired:

```text
None
```

Remove smaller:

```text
None
```

Push:

```text
5
```

Deque:

```text
5
```

Window complete?

```text
No
```

---

## Iteration 1

Current:

```text
2
```

Expired?

```text
No
```

Remove smaller?

```text
5 <= 2

No
```

Push:

```text
2
```

Deque:

```text
5 2
```

Window complete?

```text
No
```

---

## Iteration 2

Current:

```text
4
```

Expired?

```text
No
```

Remove smaller?

```text
2 <= 4

Yes

Remove 2
```

Again:

```text
5 <= 4

No
```

Push:

```text
4
```

Deque:

```text
5 4
```

Window formed?

Yes.

Maximum:

```text
5
```

Answer:

```text
5
```

---

## Iteration 3

Current:

```text
1
```

Current window:

```text
2 4 1
```

Remove expired:

```text
Remove 5
```

Deque:

```text
4
```

Remove smaller?

```text
4 <= 1

No
```

Push:

```text
1
```

Deque:

```text
4 1
```

Maximum:

```text
4
```

Answer:

```text
5 4
```

---

## Iteration 4

Current:

```text
7
```

Remove expired?

```text
No
```

Remove smaller?

```text
1 <= 7

Remove
```

Deque:

```text
4
```

Again:

```text
4 <= 7

Remove
```

Deque:

```text
Empty
```

Push:

```text
7
```

Deque:

```text
7
```

Maximum:

```text
7
```

Final Answer:

```text
5 4 7
```

---

# IMPORTANT OBSERVATIONS

### Observation 1

The **deque is NOT the sliding window**.

The window always contains:

```text
k elements
```

The deque stores only:

```text
Useful candidates for the maximum
```

---

### Observation 2

The deque size changes.

The window size never changes.

Example:

Window:

```text
5 2 4
```

Deque:

```text
5 4
```

---

### Observation 3

Every iteration represents:

```text
One new element entering the sliding window.
```

---

### Observation 4

The window is **never stored explicitly**.

It is always determined by:

```text
[i-k+1 ... i]
```

---

### Observation 5

Every element can leave the deque in only **two ways**.

**Case 1:** It leaves the window.

```text
pop_front()
```

**Case 2:** A larger element arrives.

```text
pop_back()
```

There is **no third possibility**.

---

# IMPORTANT CODE SNIPPETS

## 1. Remove Expired Elements

```cpp
while(!dq.empty() && dq.front() <= i-k)
    dq.pop_front();
```

**Remember it as:**

> **Remove elements that have left the current window.**

---

## 2. Remove Smaller Elements

```cpp
while(!dq.empty() && arr[dq.back()] <= arr[i])
    dq.pop_back();
```

**Remember it as:**

> **Remove all smaller elements because the current larger element will always dominate them.**

---

## 3. Insert Current Element

```cpp
dq.push_back(i);
```

**Remember it as:**

> **The current element has entered the window. Add it as a future candidate.**

---

## 4. Store Answer

```cpp
if(i >= k-1)
    ans.push_back(arr[dq.front()]);
```

**Remember it as:**

> **Once the first complete window exists, the front is always the maximum.**

---

# COMMON MISTAKES

## Mistake 1: Storing Values Instead of Indices

❌ Wrong

```cpp
deque<int> dq;
dq.push_back(arr[i]);
```

✅ Correct

```cpp
dq.push_back(i);
```

### Why?

We need indices to know whether an element has gone outside the current window.

---

## Mistake 2: Forgetting `()`

❌ Wrong

```cpp
dq.empty
```

✅ Correct

```cpp
dq.empty()
```

Remember:

`empty()` is a function.

---

## Mistake 3: Using `if` Instead of `while`

❌ Wrong

```cpp
if(dq.front() <= i-k)
```

✅ Correct

```cpp
while(dq.front() <= i-k)
```

Although only one element usually expires per iteration, using `while` is the standard monotonic deque template and works safely for generalized cases.

---

## Mistake 4: Pushing Before Removing Smaller Elements

❌ Wrong Flow

```text
Push

↓

Remove Smaller
```

✅ Correct Flow

```text
Remove Expired

↓

Remove Smaller

↓

Push Current
```

---

## Mistake 5: Thinking the Deque Stores the Entire Window

It doesn't.

The deque stores **only useful candidates** for the maximum.

---

## Mistake 6: Misunderstanding `if(i >= k-1)`

It does **not** check the deque size.

It checks:

> **Have I processed at least `k` elements so that the first complete window exists?**

---

# WHY I MIGHT FORGET THIS

Most students confuse these two concepts.

## Window

```text
Always exactly k elements.
```

The window is **never stored explicitly**.

---

## Deque

```text
Stores only useful candidates.
```

Its size changes continuously.

This is the biggest source of confusion.

---

# INTERVIEW FLOW

If the interviewer asks:

> **"Explain your solution."**

Follow this order.

---

## Step 1: Brute Force

> For every window, scan all `k` elements and find the maximum.

Time Complexity:

```text
O(nk)
```

---

## Step 2: Optimization

Observation:

Most elements are scanned repeatedly when the window slides.

We need to avoid rescanning them.

---

## Step 3: Key Observation

Any element smaller than a newly arrived larger element can **never become the maximum**.

Therefore,

remove it.

---

## Step 4: Data Structure

Use a **Monotonically Decreasing Deque**.

Store **indices**, not values.

---

## Step 5: Algorithm

For every new element:

- Remove expired indices.
- Remove smaller elements.
- Push current index.
- Once the first window is formed, the front gives the maximum.

---

## Step 6: Complexity

Each element is:

```text
Pushed once

Popped at most once
```

Therefore,

```text
O(n)
```

---

# TIME COMPLEXITY

Many students think

```text
For

↓

While

↓

While
```

means

```text
O(n²)
```

It is **NOT**.

---

## Derivation

### 1. For Loop

Runs

```text
n
```

times.

---

### 2. Push

Every element is inserted exactly once.

```text
n pushes
```

---

### 3. pop_front()

Every element can leave the window only once.

Maximum:

```text
n
```

operations.

---

### 4. pop_back()

Every element can be removed by a larger element only once.

Maximum:

```text
n
```

operations.

---

### Total Operations

```text
n

+

n

+

n

+

n

=

4n
```

Ignoring constants,

```text
O(n)
```

---

# WHY EACH ELEMENT IS PUSHED AND POPPED AT MOST ONCE

Suppose

```text
5
```

is inserted.

There are only **two possibilities**.

---

## Case 1

A larger element arrives.

```text
5

↓

pop_back()
```

Gone forever.

---

## Case 2

No larger element arrives.

Eventually,

```text
5
```

leaves the window.

```text
↓

pop_front()
```

Gone forever.

Therefore,

every element contributes at most

```text
One Push

+

One Pop
```

This is why the total number of deque operations is linear.

---

# AMORTIZED O(n)

One iteration may remove many elements.

Example:

Deque

```text
9 7 5 3
```

Incoming element

```text
10
```

Removes

```text
3

5

7

9
```

Looks expensive.

But notice:

Those elements are removed **forever**.

Future iterations never touch them again.

Although one iteration performs many operations, the **total** work across the entire algorithm remains linear.

This is called

```text
Amortized O(n)
```

---

# SPACE COMPLEXITY

The deque stores

```text
Indices
```

Maximum size?

At most

```text
k
```

because it only contains indices from the current window.

Therefore,

```text
Auxiliary Space = O(k)
```

(Output array excluded.)

---

# EDGE CASES

## 1. k = 1

Every element is its own window.

Answer = Original array.

---

## 2. k = n

Only one window exists.

Answer = Maximum element of the entire array.

---

## 3. Strictly Increasing Array

```text
1 2 3 4 5
```

Deque always contains only

```text
One element
```

because every new element removes all previous ones.

---

## 4. Strictly Decreasing Array

```text
5 4 3 2 1
```

Deque may contain

```text
k elements
```

Still,

```text
O(n)
```

---

## 5. Duplicate Elements

Use

```cpp
<=
```

instead of

```cpp
<
```

This removes older duplicates and keeps the newest one, which stays valid in the window longer.

---

# PATTERN RECOGNITION

Whenever you see:

- Sliding Window
- Fixed-size window
- Maximum/Minimum in every window
- Maintain best candidate while the window moves
- First larger/smaller within a moving window

Think:

```text
Sliding Window

+

Monotonic Deque
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    vector<int> maxOfSubarrays(vector<int>& arr, int k) {

        int n = arr.size();

        vector<int> ans;
        deque<int> dq;

        for(int i = 0; i < n; i++) {

            // Remove expired indices
            while(!dq.empty() && dq.front() <= i-k)
                dq.pop_front();

            // Remove smaller elements
            while(!dq.empty() && arr[dq.back()] <= arr[i])
                dq.pop_back();

            // Insert current index
            dq.push_back(i);

            // First complete window formed
            if(i >= k-1)
                ans.push_back(arr[dq.front()]);
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
deque<int> dq;
```

Maintain only the useful candidates for the maximum.

---

```cpp
while(dq.front() <= i-k)
```

Remove elements that are outside the current window.

---

```cpp
while(arr[dq.back()] <= arr[i])
```

Remove elements that can never become the maximum again because a larger element has arrived.

---

```cpp
dq.push_back(i);
```

The current element becomes a new candidate for the maximum.

---

```cpp
if(i >= k-1)
```

The first complete window has been formed.

---

```cpp
arr[dq.front()]
```

The front always stores the maximum because the deque is maintained in decreasing order.

---

# EASY-TO-REMEMBER SUMMARY

Every iteration follows the same flow:

```text
A new element enters

↓

Remove expired elements

↓

Remove useless smaller elements

↓

Insert current element

↓

If a complete window exists,
the front is the maximum

↓

Store answer
```

---

# THE ONE MENTAL MODEL TO REMEMBER

Don't memorize the code.

Read it like English.

```cpp
while(front expired)
```

↓

> Remove elements that have left the current window.

---

```cpp
while(back smaller)
```

↓

> Remove elements that can never become the maximum.

---

```cpp
push_back(i)
```

↓

> Add the current element as a new candidate.

---

```cpp
if(window formed)
```

↓

> Store the maximum for the current window.

---

# ⭐ FINAL TAKEAWAY

Never forget these **three ideas**:

1. **The window is never stored.**
   It is implicitly defined as:

   ```text
   [i-k+1 ... i]
   ```

2. **The deque is NOT the window.**
   It stores only the useful candidates for the maximum while maintaining a **monotonically decreasing order**.

3. **Every element is pushed once and popped at most once.**
   This is why the overall time complexity is:

   ```text
   O(n)
   ```

