
## PROBLEM:

Given a **sorted array** (may contain duplicates), return the **first** and **last** occurrence of a target `x`.

If `x` is not present, return `{-1, -1}`.

### Problem Intuition

A normal Binary Search can tell us **whether an element exists**, but it does **not guarantee** finding its first or last occurrence.

Example:

```text
arr = [1, 3, 5, 5, 5, 5, 7]
```

A normal Binary Search may return:

```text
Index 2
or
Index 3
or
Index 4
or
Index 5
```

But the question specifically asks for:

```text
First occurrence = 2
Last occurrence = 5
```

So we need a Binary Search that finds the **boundaries**, not just any occurrence.

---

# PATTERN:

**Boundary Binary Search (Modified Binary Search)**

Also known as:

- First Occurrence Binary Search
- Last Occurrence Binary Search
- Lower Bound / Upper Bound style search

---

# WHY THIS PATTERN:

The array is **sorted**, so Binary Search can eliminate half the search space every step.

The challenge is that duplicates exist.

A normal Binary Search stops after finding the target.

Here, we must continue searching to find the **leftmost** or **rightmost** occurrence.

Instead of asking:

> "Did I find x?"

We ask:

> "Can I find another x further left/right?"

That's exactly what Boundary Binary Search is designed for.

---

# CORE IDEA:

Run Binary Search **twice**.

### First Occurrence

Whenever you find `x`:

- Store the current index.
- Continue searching on the **left**.

```cpp
ans = mid;
high = mid - 1;
```

### Last Occurrence

Whenever you find `x`:

- Store the current index.
- Continue searching on the **right**.

```cpp
ans = mid;
low = mid + 1;
```

The only difference between the two searches is **which direction you continue after finding the target**.

---

# BRUTE FORCE:

### Intuition

Ignore the fact that the array is sorted.

Simply scan every element from left to right.

- The first time you find `x`, store it as `first`.
- Every time you find `x`, update `last`.

At the end:

- `first` = first occurrence
- `last` = last occurrence

### Code

```cpp
vector<int> find(vector<int>& arr, int x) {

    int first = -1;
    int last = -1;

    for (int i = 0; i < arr.size(); i++) {

        if (arr[i] == x) {

            if (first == -1)
                first = i;

            last = i;
        }
    }

    return {first, last};
}
```

### Dry Run

```text
arr = [1,3,5,5,5,5,7]
x = 5

i=0 → not found

i=1 → not found

i=2 → first=2 last=2

i=3 → last=3

i=4 → last=4

i=5 → last=5

Answer = {2,5}
```

### Why Optimize?

This solution completely ignores the fact that the array is sorted.

Even after finding the target, it still checks every remaining element.

Since the array is sorted, we should use Binary Search to avoid unnecessary work.

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(1)
```

---

# OPTIMAL APPROACH:

Perform **two modified Binary Searches**.

### Search 1

Find the first occurrence.

Whenever `arr[mid] == x`:

- Save `mid`.
- Continue searching left.

### Search 2

Find the last occurrence.

Whenever `arr[mid] == x`:

- Save `mid`.
- Continue searching right.

Finally return:

```text
{first, last}
```

---

# ALGORITHM:

### First Occurrence

```text
Initialize:

low = 0
high = n-1
ans = -1

while(low <= high)

    mid

    if(arr[mid] == x)

        ans = mid
        high = mid - 1

    else if(arr[mid] < x)

        low = mid + 1

    else

        high = mid - 1
```

### Last Occurrence

```text
Initialize:

low = 0
high = n-1
ans = -1

while(low <= high)

    mid

    if(arr[mid] == x)

        ans = mid
        low = mid + 1

    else if(arr[mid] < x)

        low = mid + 1

    else

        high = mid - 1
```

Return

```text
{first, last}
```

---

# DRY RUN:

```text
arr = [1,3,5,5,5,5,7]
Target = 5
```

### First Occurrence

```text
low=0 high=6

mid=3

arr[3]=5

ans=3

move left

high=2
```

```text
low=0 high=2

mid=1

arr[1]=3

move right

low=2
```

```text
low=2 high=2

mid=2

arr[2]=5

ans=2

move left

high=1

stop
```

First = 2

### Last Occurrence

```text
low=0 high=6

mid=3

arr[3]=5

ans=3

move right

low=4
```

```text
low=4 high=6

mid=5

arr[5]=5

ans=5

move right

low=6
```

```text
low=6 high=6

mid=6

arr[6]=7

move left

high=5

stop
```

Last = 5

Answer

```text
{2,5}
```

---

# IMPORTANT CODE SNIPPETS:

### First Occurrence

```cpp
if (arr[mid] == x) {
    ans = mid;
    high = mid - 1;
}
```

### Last Occurrence

```cpp
if (arr[mid] == x) {
    ans = mid;
    low = mid + 1;
}
```

### Safe Mid Calculation

```cpp
int mid = low + (high - low) / 2;
```

---

# COMMON MISTAKES:

### Mistake 1

Stopping immediately after finding the target.

You must continue searching for the boundary.

### Mistake 2

Moving in the wrong direction.

For first occurrence:

```cpp
high = mid - 1;
```

For last occurrence:

```cpp
low = mid + 1;
```

### Mistake 3

Not storing the answer before moving.

Always do:

```cpp
ans = mid;
```

before changing `low` or `high`.

### Mistake 4

Using normal Binary Search.

It only guarantees **any occurrence**, not the first or last.

### Mistake 5

Returning `{ans, ans}` after one Binary Search.

First and last boundaries are independent searches.

---

# WHY I MIGHT FORGET THIS:

Both Binary Searches look almost identical.

Remember this question:

> **After finding the target, which boundary do I want?**

If the answer is:

**First occurrence**

→ Go Left

```cpp
high = mid - 1;
```

If the answer is:

**Last occurrence**

→ Go Right

```cpp
low = mid + 1;
```

That's the only difference.

---

# INTERVIEW FLOW:

**Step 1**

The array is sorted.

So Binary Search is applicable.

**Step 2**

Duplicates exist.

A normal Binary Search cannot guarantee the first or last occurrence.

**Step 3**

Run Binary Search once for the first occurrence.

Whenever the target is found:

- Save the answer.
- Continue left.

**Step 4**

Run Binary Search again for the last occurrence.

Whenever the target is found:

- Save the answer.
- Continue right.

**Step 5**

Return both indices.

---

# TIME COMPLEXITY:

Each Binary Search takes:

```
O(log n)
```

We perform two Binary Searches.

```
O(log n) + O(log n)
```

Ignoring constant factors,

```
O(log n)
```

### Reasoning

Each Binary Search halves the search space after every comparison.

```text
n

↓

n/2

↓

n/4

↓

n/8

↓

...
```

After roughly `log₂(n)` steps, the search space becomes empty.

Running it twice only doubles the constant, not the complexity.

---

# SPACE COMPLEXITY:

Only a few integer variables are used:

- low
- high
- mid
- ans

No extra arrays or recursion.

```
O(1)
```

---

# EDGE CASES:

- Target not present

```text
arr = [1,2,3]
x = 5

{-1,-1}
```

- Only one occurrence

```text
arr = [1,2,3]
x = 2

{1,1}
```

- Entire array is the target

```text
5 5 5 5

{0,3}
```

- Target at beginning

```text
5 5 6 7

{0,1}
```

- Target at end

```text
1 2 3 5 5

{3,4}
```

- Single element present

```text
[5]

{0,0}
```

- Single element absent

```text
[3]

{-1,-1}
```

---

# PATTERN RECOGNITION:

Think of **Boundary Binary Search** whenever the problem contains phrases like:

- First occurrence
- Last occurrence
- Leftmost index
- Rightmost index
- Lower Bound
- Upper Bound
- Search Insert Position
- Find Range of Element
- Count occurrences in sorted array
- First element satisfying a condition
- Last element satisfying a condition

### Golden Rule

Need **left boundary**?

```text
Found target

↓

Store answer

↓

Move LEFT
```

```cpp
high = mid - 1;
```

Need **right boundary**?

```text
Found target

↓

Store answer

↓

Move RIGHT
```

```cpp
low = mid + 1;
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int firstOccurrence(vector<int>& arr, int x) {
        int low = 0, high = arr.size() - 1;
        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == x) {
                ans = mid;
                high = mid - 1;
            }
            else if (arr[mid] < x) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    int lastOccurrence(vector<int>& arr, int x) {
        int low = 0, high = arr.size() - 1;
        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == x) {
                ans = mid;
                low = mid + 1;
            }
            else if (arr[mid] < x) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    vector<int> find(vector<int>& arr, int x) {
        return {firstOccurrence(arr, lastOccurrence(arr, x)};
    }
};
```

> **Note:** The last line above contains a typo. It should be:

```cpp
return {firstOccurrence(arr, x), lastOccurrence(arr, x)};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int ans = -1;
```

Assume the element does not exist.

---

```cpp
int mid = low + (high - low) / 2;
```

Safely computes the middle index.

---

```cpp
if (arr[mid] == x)
```

We found the target, but continue searching because we need a boundary.

---

```cpp
ans = mid;
```

Store the current valid occurrence.

---

```cpp
high = mid - 1;
```

Search further left for the first occurrence.

---

```cpp
low = mid + 1;
```

Search further right for the last occurrence.

---

```cpp
else if (arr[mid] < x)
```

Target must lie on the right.

---

```cpp
else
```

Target must lie on the left.

---

# EASY-TO-REMEMBER SUMMARY

- **Sorted array + first/last occurrence = Boundary Binary Search**
- Run Binary Search **twice**
- **First occurrence → Found → Go LEFT**
- **Last occurrence → Found → Go RIGHT**
- Never stop immediately after finding the target.
- The only difference between the two searches is the direction after finding `x`.
