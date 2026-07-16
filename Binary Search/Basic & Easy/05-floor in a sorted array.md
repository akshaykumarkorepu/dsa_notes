# Floor in a Sorted Array

## PROBLEM:

Given a **sorted array** and an integer `x`, return the **index (0-based) of the largest element that is less than or equal to `x`**.

- If multiple occurrences exist, return the **last occurrence**.
- If no such element exists, return **-1**.

### Example

```text
arr = [1, 2, 8, 10, 10, 12, 19]
x = 11

Answer = 4
```

Because `10` is the floor of `11`, and its last occurrence is at index `4`.

---

# PATTERN:

**Last True Binary Search (Upper Bound - 1)**

### Trigger

> "Find the last element satisfying a condition in a sorted array."

---

# WHY THIS PATTERN:

Since the array is sorted,

```text
arr[i] <= x   → Valid
arr[i] > x    → Invalid
```

The array always forms a pattern like:

```text
✔ ✔ ✔ ✔ ✔ ✘ ✘ ✘
```

We need to find the **last valid element**, which is exactly what Binary Search on a boundary is designed for.

---

# CORE IDEA:

Whenever

```cpp
arr[mid] <= x
```

Current element is a valid floor.

Store it and continue searching on the **right** because there may be a larger valid element.

```cpp
ans = mid;
low = mid + 1;
```

Whenever

```cpp
arr[mid] > x
```

Current element cannot be the floor.

Search on the **left**.

```cpp
high = mid - 1;
```

At the end, `ans` stores the last valid index.

---

# BRUTE FORCE:

## Idea

Traverse the entire array.

Whenever an element is `<= x`, update the answer.

The last updated index will be the floor.

### Code

```cpp
int findFloor(vector<int>& arr, int x) {
    int ans = -1;

    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] <= x)
            ans = i;
    }

    return ans;
}
```

### Dry Run

```text
arr = [1,2,8,10,10,12]
x = 11

1  -> ans = 0
2  -> ans = 1
8  -> ans = 2
10 -> ans = 3
10 -> ans = 4
12 -> ignore

Answer = 4
```

### Time Complexity

```text
O(N)
```

### Space Complexity

```text
O(1)
```

---

# OPTIMAL APPROACH:

Use Binary Search.

Maintain an answer variable.

Whenever a valid floor is found:

- Save it.
- Continue searching on the right.

Whenever the value becomes greater than `x`:

- Search on the left.

Eventually, the stored answer becomes the last valid index.

---

# ALGORITHM:

```text
ans = -1

low = 0
high = n - 1

while(low <= high)

    mid = low + (high-low)/2

    if(arr[mid] <= x)

        ans = mid
        low = mid + 1

    else

        high = mid - 1

return ans
```

---

# DRY RUN:

### Example

```text
arr = [1,2,8,10,10,12,19]
x = 11
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

10 <= 11 ✔
```

Store answer

```text
ans = 3
low = 4
```

---

### Iteration 2

```text
low = 4
high = 6

mid = 5

arr[5] = 12
```

Too large.

```text
high = 4
```

---

### Iteration 3

```text
low = 4
high = 4

mid = 4

arr[4] = 10
```

Valid.

```text
ans = 4
low = 5
```

Loop ends.

Return

```text
4
```

---

### Another Example

```text
arr = [1,2,8,10]
x = 0
```

```text
mid = 1

2 > 0

high = 0

mid = 0

1 > 0

high = -1
```

No valid answer found.

Return

```text
-1
```

---

# IMPORTANT CODE SNIPPETS:

## Last Valid Element

```cpp
if(arr[mid] <= x)
{
    ans = mid;
    low = mid + 1;
}
```

---

## Invalid Element

```cpp
else
{
    high = mid - 1;
}
```

---

## Binary Search Loop

```cpp
while(low <= high)
```

---

## Safe Mid Calculation

```cpp
int mid = low + (high - low) / 2;
```

---

# COMMON MISTAKES:

### 1. Returning immediately when equal

Wrong

```cpp
if(arr[mid] == x)
    return mid;
```

Fails for duplicates.

Need the **last occurrence**.

---

### 2. Moving left after finding a valid answer

Wrong

```cpp
high = mid - 1;
```

You might miss a larger valid floor.

Always move right.

---

### 3. Forgetting the answer variable

Without storing `ans`, you lose previously found valid indices.

---

### 4. Returning the value instead of the index

The problem asks for the **index**, not the element.

---

### 5. Using `< x` instead of `<= x`

Exact matches must also be considered floors.

---

# WHY I MIGHT FORGET THIS:

This looks like normal Binary Search.

But the real question is:

> **Find the Last True.**

Whenever the condition is true:

```text
Save Answer
Move Right
```

This single rule solves almost every floor problem.

---

# INTERVIEW FLOW:

> The array is sorted, so Binary Search is applicable.

> Every element `<= x` is valid, while every element `> x` is invalid.

> This creates a monotonic True → False boundary.

> Since we need the **last valid element**, whenever `arr[mid] <= x`, I store `mid` as the current answer and continue searching on the right.

> Otherwise, I search on the left.

> Finally, I return the stored answer, or `-1` if no valid element exists.

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

N/8
↓

...
```

Number of iterations:

```text
log₂N
```

Each iteration performs constant work.

### Overall

```text
Time = O(log N)
```

---

# SPACE COMPLEXITY:

Only these variables are used:

- low
- high
- mid
- ans

### Overall

```text
O(1)
```

---

# EDGE CASES:

### No floor exists

```text
arr = [5,6,7]
x = 3

Answer = -1
```

---

### Exact match

```text
arr = [1,2,5]
x = 5

Answer = 2
```

---

### Duplicate floor

```text
arr = [1,2,10,10,10,15]
x = 11

Answer = 4
```

---

### x greater than every element

```text
arr = [1,3,5]
x = 100

Answer = 2
```

---

### Single element

```text
arr = [5]
x = 5

Answer = 0
```

---

# PATTERN RECOGNITION:

Look for phrases like:

- Largest element ≤ x
- Floor in sorted array
- Rightmost element ≤ target
- Last occurrence satisfying a condition
- Upper Bound - 1
- Last True Binary Search
- Boundary Search

### Mental Template

```text
Need Last Valid?

↓

Binary Search

↓

Condition True?

↓

Store Answer

↓

Move Right

↓

Condition False?

↓

Move Left
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int findFloor(vector<int>& arr, int x) {

        int low = 0;
        int high = arr.size() - 1;

        int ans = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (arr[mid] <= x) {
                ans = mid;          // Current element is a valid floor
                low = mid + 1;      // Try to find a larger valid floor
            }
            else {
                high = mid - 1;     // Floor must be on the left
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

Assume no floor exists initially.

---

### Binary Search loop

```cpp
while(low <= high)
```

Keep searching while a valid search space exists.

---

### Safe mid calculation

```cpp
int mid = low + (high - low) / 2;
```

Avoids integer overflow.

---

### Check validity

```cpp
if(arr[mid] <= x)
```

Current element can be the floor.

---

### Save answer

```cpp
ans = mid;
```

Remember the best floor found so far.

---

### Search right

```cpp
low = mid + 1;
```

A larger valid floor may still exist.

---

### Search left

```cpp
high = mid - 1;
```

Current element is too large.

Discard the right half.

---

### Return answer

```cpp
return ans;
```

Returns the index of the last valid floor or `-1`.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Last True Binary Search (Upper Bound - 1)
- **Condition:** `arr[mid] <= x`
- **If condition is true:** Store answer and move right.
- **If condition is false:** Move left.
- **Answer:** Last stored valid index.
