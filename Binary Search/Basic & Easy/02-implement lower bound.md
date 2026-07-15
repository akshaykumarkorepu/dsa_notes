
## PROBLEM:

Given a **sorted array** `arr[]` and a **target**, return the **smallest index** where:

```text
arr[index] >= target
```

If no such index exists, return `n` (the length of the array).

This index is called the **Lower Bound**.

---

## PATTERN:

**Binary Search (First True / Lower Bound Pattern)**

---

## WHY THIS PATTERN:

The array is **sorted**, so the condition

```text
arr[i] >= target
```

forms a monotonic pattern.

Example:

```text
arr = [2,3,7,10,11,11,25]
target = 9

2   >= 9 ?  False
3   >= 9 ?  False
7   >= 9 ?  False
10  >= 9 ?  True
11  >= 9 ?  True
11  >= 9 ?  True
25  >= 9 ?  True
```

This becomes

```text
F F F T T T T
      ↑
 First True
```

Whenever the answer changes from **False → True**, Binary Search can efficiently find the **first True**.

---

## CORE IDEA:

Don't think:

> "I need to find the target."

Think:

> "I need to find the first index where the condition becomes true."

Condition:

```text
arr[mid] >= target
```

- If **True**
  - `mid` is a possible answer.
  - Save it.
  - Search left for an earlier valid index.

- If **False**
  - `mid` and everything before it are too small.
  - Search right.

---

## BRUTE FORCE:

### Idea

Traverse the array from left to right.

Return the first index where

```text
arr[i] >= target
```

If no such element exists, return `n`.

### Code

```cpp
int lowerBound(vector<int>& arr, int target) {
    int n = arr.size();

    for (int i = 0; i < n; i++) {
        if (arr[i] >= target)
            return i;
    }

    return n;
}
```

### Dry Run

```text
arr = [2,3,7,10,11]
target = 9

2 < 9
3 < 9
7 < 9
10 >= 9

Answer = 3
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

## OPTIMAL APPROACH:

Use Binary Search.

Maintain an answer variable initialized as `n`.

At every step,

- If `arr[mid] >= target`
  - store `mid` as a possible answer.
  - search the left half for a smaller valid index.

- Otherwise
  - discard the left half.
  - search the right half.

Finally, return the stored answer.

---

## ALGORITHM:

```text
Initialize:

low = 0
high = n-1
ans = n

while(low <= high)

    mid = low + (high-low)/2

    if(arr[mid] >= target)

         ans = mid
         high = mid - 1

    else

         low = mid + 1

return ans
```

---

## DRY RUN:

### Example

```text
arr = [2,3,7,10,11,11,25]
target = 9
```

Initially

```text
low = 0
high = 6
ans = 7
```

### Iteration 1

```text
mid = (0+6)/2 = 3

arr[mid] = 10
```

Question:

```text
Is 10 >= 9 ?

Yes
```

Index 3 can be the answer.

```text
ans = 3
```

But maybe there is an earlier valid index.

Search left.

```text
high = 2
```

Current state

```text
low = 0
high = 2
ans = 3
```

---

### Iteration 2

```text
mid = (0+2)/2 = 1

arr[mid] = 3
```

Question

```text
Is 3 >= 9 ?

No
```

3 and everything before it are too small.

Move right.

```text
low = 2
```

Current state

```text
low = 2
high = 2
ans = 3
```

---

### Iteration 3

```text
mid = 2

arr[mid] = 7
```

Question

```text
Is 7 >= 9 ?

No
```

Again,

7 and everything before it are too small.

Move right.

```text
low = 3
```

Now

```text
low = 3
high = 2
```

Loop ends.

Return

```text
ans = 3
```

Correct answer.

---

## IMPORTANT OBSERVATIONS:

- We are **not searching for the target**.
- We are searching for the **first index satisfying a condition**.
- Every valid index is a possible answer.
- Even after finding one valid index, continue searching left.
- If no valid index exists, return `n`.

---

## IMPORTANT CODE SNIPPETS:

### Initialize answer

```cpp
int ans = n;
```

### Safe middle calculation

```cpp
int mid = low + (high - low) / 2;
```

### Valid condition

```cpp
if(arr[mid] >= target)
```

### Save current answer

```cpp
ans = mid;
```

### Search left

```cpp
high = mid - 1;
```

### Search right

```cpp
low = mid + 1;
```

---

## COMMON MISTAKES:

### Mistake 1

Searching for equality.

```cpp
if(arr[mid] == target)
```

Wrong.

Always check

```cpp
arr[mid] >= target
```

---

### Mistake 2

Returning immediately after finding a valid element.

Wrong.

A smaller valid index may exist.

Always search left.

---

### Mistake 3

Initializing

```cpp
ans = -1;
```

Wrong.

If no answer exists, we must return `n`.

---

### Mistake 4

Moving right after finding a valid answer.

Wrong

```cpp
low = mid + 1;
```

Correct

```cpp
high = mid - 1;
```

because we want the **first valid index**.

---

## WHY I MIGHT FORGET THIS:

Most people think Binary Search always means

> "Find the target."

This question is different.

We are finding

> **The first index satisfying a condition.**

Remember this picture:

```text
F F F T T T T
      ↑
 First True
```

Lower Bound = First True.

---

## INTERVIEW FLOW:

**Step 1**

The array is sorted, so Binary Search is applicable.

**Step 2**

Instead of searching for the target, define the condition:

```text
arr[i] >= target
```

**Step 3**

This condition forms a monotonic sequence.

```text
False False False True True True
```

The answer is the first True.

**Step 4**

If

```text
arr[mid] >= target
```

store `mid` as a possible answer and search left.

Otherwise search right.

**Step 5**

If no valid index is found, the answer remains `n`.

---

## TIME COMPLEXITY:

Each iteration cuts the search space into half.

```text
N
N/2
N/4
N/8
...
```

Number of iterations:

```text
log₂N
```

Therefore,

```text
Time Complexity = O(log N)
```

---

## SPACE COMPLEXITY:

Only a few variables are used.

```text
Space Complexity = O(1)
```

---

## EDGE CASES:

### Target smaller than every element

```text
arr = [5,7,9]
target = 2

Answer = 0
```

### Target larger than every element

```text
arr = [2,3,5]
target = 10

Answer = n
```

### Target equals first element

```text
arr = [5,7,8]
target = 5

Answer = 0
```

### Duplicate elements

```text
arr = [2,4,4,4,8]
target = 4

Answer = 1
```

(Return the first occurrence.)

### Single element

```text
arr = [5]
target = 5

Answer = 0
```

---

## PATTERN RECOGNITION:

Think of this pattern whenever you see:

- Sorted array
- Lower Bound
- Search Insert Position
- First occurrence
- First element ≥ X
- Minimum index satisfying a condition
- First True in a monotonic sequence

**Mental trigger:**

> **Binary Search on "First True".**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int lowerBound(vector<int>& arr, int target) {
        int n = arr.size();

        int low = 0;
        int high = n - 1;
        int ans = n;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] >= target) {
                ans = mid;          // Possible lower bound
                high = mid - 1;     // Search for an earlier valid index
            } else {
                low = mid + 1;      // Current value is too small
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int ans = n;
```

Assume no lower bound exists. If we never find a valid index, returning `n` is correct.

---

```cpp
while (low <= high)
```

Continue searching until the search space becomes empty.

---

```cpp
int mid = low + (high - low) / 2;
```

Find the middle index safely without integer overflow.

---

```cpp
if (arr[mid] >= target)
```

Ask the only question that matters:

> **Is this index valid?**

---

```cpp
ans = mid;
```

Current index satisfies the condition, so store it as a possible answer.

---

```cpp
high = mid - 1;
```

Look for an even earlier valid index because we want the **first** one.

---

```cpp
low = mid + 1;
```

Current value is too small, so discard the left half and search right.

---

```cpp
return ans;
```

Return the first index where `arr[index] >= target`, or `n` if no such index exists.

---

# Easy-to-Remember Summary

- Lower Bound = **First index where `arr[i] >= target`**
- Think in terms of **conditions**, not values.
- Condition:

```text
arr[mid] >= target
```

- **If True**
  - Save `mid`
  - Go Left

- **If False**
  - Go Right

Visualize it as:

```text
F F F T T T T
      ↑
 First True
```

**One-line Memory Trick:**

> **Lower Bound = Binary Search for the First True.**
