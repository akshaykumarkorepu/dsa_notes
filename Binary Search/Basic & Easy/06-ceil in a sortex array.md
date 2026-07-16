

## PROBLEM:

Given a **sorted array** and an integer `x`, find the **index (0-based) of the smallest element greater than or equal to `x`**.

- If multiple occurrences exist, return the **first occurrence**.
- If no such element exists, return **-1**.

### Example

```text
arr = [1,2,8,10,11,12,19]
x = 5

Answer = 2
```

Because `8` is the smallest element that is `>= 5`.

---

# PATTERN:

**First True Binary Search (Lower Bound)**

**Trigger:**

> "Find the first/leftmost element satisfying a condition in a sorted array."

---

# WHY THIS PATTERN:

Since the array is sorted,

```text
arr[i] < x     → Invalid
arr[i] >= x    → Valid
```

The array always forms a pattern like

```text
✘ ✘ ✘ ✔ ✔ ✔ ✔
```

There is a clear boundary between invalid and valid elements.

Binary Search is ideal for finding this **first valid position**.

---

# CORE IDEA:

Whenever

```cpp
arr[mid] >= x
```

the current element is a valid ceil.

Store its index because it could be the answer.

But there might be another valid element on the **left**.

So,

```cpp
ans = mid;
high = mid - 1;
```

Whenever

```cpp
arr[mid] < x
```

the current element cannot be the answer.

Search on the **right**.

```cpp
low = mid + 1;
```

At the end, `ans` stores the first valid index.

---

# BRUTE FORCE:

### Idea

Traverse from left to right.

The **first** element satisfying

```cpp
arr[i] >= x
```

is the answer.

Immediately return its index.

### Code

```cpp
int findCeil(vector<int>& arr, int x) {

    for(int i = 0; i < arr.size(); i++)
    {
        if(arr[i] >= x)
            return i;
    }

    return -1;
}
```

### Dry Run

```text
arr = [1,2,8,10]

x = 5

1 < 5

2 < 5

8 >= 5

Return 2
```

### Complexity

```text
Time : O(N)

Space : O(1)
```

---

# OPTIMAL APPROACH:

Use Binary Search.

Maintain an answer variable.

Whenever a valid ceil is found,

store it,

then continue searching on the **left**.

Whenever the value is smaller than `x`,

move right.

Eventually,

the stored answer becomes the first valid index.

---

# ALGORITHM:

```text
ans = -1

low = 0
high = n - 1

while(low <= high)

    mid = low + (high-low)/2

    if(arr[mid] >= x)

        ans = mid

        high = mid - 1

    else

        low = mid + 1

return ans
```

---

# DRY RUN:

### Example

```text
arr = [1,2,8,10,11,12,19]

x = 5
```

Initial

```text
low = 0
high = 6
ans = -1
```

### Iteration 1

```text
mid = 3

arr[3] = 10

10 >= 5 ✔
```

Store answer.

```text
ans = 3

high = 2
```

---

### Iteration 2

```text
low = 0
high = 2

mid = 1

arr[1] = 2

2 < 5
```

Search right.

```text
low = 2
```

---

### Iteration 3

```text
low = 2
high = 2

mid = 2

arr[2] = 8

8 >= 5
```

Store answer.

```text
ans = 2

high = 1
```

Loop ends.

Return

```text
2
```

Correct.

---

### Another Example

```text
arr = [1,2,8]

x = 20
```

Every element is smaller.

`ans` never updates.

Return

```text
-1
```

---

# IMPORTANT CODE SNIPPETS:

### First Valid Element

```cpp
if(arr[mid] >= x)
{
    ans = mid;
    high = mid - 1;
}
```

---

### Invalid Element

```cpp
else
{
    low = mid + 1;
}
```

---

### Binary Search Loop

```cpp
while(low <= high)
```

---

### Safe Mid Calculation

```cpp
int mid = low + (high - low) / 2;
```

---

# COMMON MISTAKES:

### 1. Returning immediately when `arr[mid] == x`

Wrong

```cpp
if(arr[mid] == x)
    return mid;
```

Fails for duplicates.

Need the **first occurrence**.

---

### 2. Moving right after finding a valid answer

Wrong

```cpp
low = mid + 1;
```

You may miss a smaller valid ceil.

Always move left.

---

### 3. Forgetting the answer variable

Without `ans`, the previously found valid candidate is lost.

---

### 4. Returning the value instead of the index

The problem asks for the **index**, not the element.

---

### 5. Using `>` instead of `>=`

Exact matches are also valid ceils.

---

# WHY I MIGHT FORGET THIS:

This looks like a normal Binary Search.

But the real problem is

> **Find the First True.**

Condition

```cpp
arr[mid] >= x
```

When the condition is true,

```text
Store Answer

Move Left
```

This one rule solves almost every **Ceil / Lower Bound** problem.

---

# INTERVIEW FLOW:

> The array is sorted, so Binary Search is applicable.

> I need the first element satisfying `arr[i] >= x`.

> Whenever `arr[mid] >= x`, I store it as the current answer and continue searching left.

> If `arr[mid] < x`, I search the right half.

> The stored answer is the ceil index, or `-1` if no valid element exists.

---

# TIME COMPLEXITY:

Binary Search halves the search space every iteration.

```text
N

↓

N/2

↓

N/4

↓

...
```

Number of iterations

```text
log₂N
```

Each iteration performs constant work.

Overall

```text
O(log N)
```

---

# SPACE COMPLEXITY:

Only

- `low`
- `high`
- `mid`
- `ans`

are used.

Overall

```text
O(1)
```

---

# EDGE CASES:

### No ceil exists

```text
arr = [1,2,3]

x = 10

Answer = -1
```

---

### Exact match

```text
arr = [1,3,5]

x = 3

Answer = 1
```

---

### Duplicate ceil

```text
arr = [1,1,1,5,7]

x = 1

Answer = 0
```

---

### x smaller than every element

```text
arr = [5,7,8]

x = 2

Answer = 0
```

---

### Single element

```text
arr = [5]

x = 3

Answer = 0
```

---

# PATTERN RECOGNITION:

Look for phrases like:

- Smallest element ≥ x
- Ceil in sorted array
- First occurrence satisfying a condition
- Leftmost element ≥ target
- Lower Bound
- First True Binary Search
- Boundary Search

### Mental Template

```text
Need First Valid?

↓

Binary Search

↓

Condition True?

↓

Store Answer

↓

Move Left

↓

Condition False?

↓

Move Right
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int findCeil(vector<int>& arr, int x) {

        int low = 0;
        int high = arr.size() - 1;

        int ans = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (arr[mid] >= x) {
                ans = mid;          // Current element is a valid ceil
                high = mid - 1;     // Try to find a smaller valid ceil
            }
            else {
                low = mid + 1;      // Ceil must be on the right
            }
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Initialize answer

```cpp
int ans = -1;
```

Assume no ceil exists initially.

---

### Binary Search loop

```cpp
while(low <= high)
```

Continue searching until the search space becomes empty.

---

### Safe mid calculation

```cpp
int mid = low + (high - low) / 2;
```

Safely compute the middle index.

---

### Check validity

```cpp
if(arr[mid] >= x)
```

Current element can be the ceil.

---

### Save answer

```cpp
ans = mid;
```

Remember the best valid answer found so far.

---

### Search left

```cpp
high = mid - 1;
```

A smaller valid ceil may still exist on the left.

---

### Search right

```cpp
low = mid + 1;
```

Current value is too small.

Discard the left half.

---

### Return answer

```cpp
return ans;
```

Returns the index of the first valid ceil, or `-1` if none exists.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** First True Binary Search (Lower Bound)
- **Condition:** `arr[mid] >= x`
- **If condition is true:** Store answer and move **left**.
- **If condition is false:** Move **right**.
- **Answer:** First stored valid index.
