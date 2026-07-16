# Implement Upper Bound (Binary Search)

## PROBLEM:

Given a **sorted array**, return the **smallest index** where the element is **strictly greater than `target`**.

If no such element exists, return `n` (length of the array).

This is called the **Upper Bound**.

### Example

```text
arr = [2,3,7,10,11,11,25]
target = 11

Answer = 6
Because arr[6] = 25 is the first element > 11.
```

---

## PATTERN:

**Binary Search on Answer**

**Trigger:**
> Find the first element/index satisfying a condition.

---

## WHY THIS PATTERN:

The array is sorted.

Observe:

```text
target = 11

Index : 0 1 2 3 4 5 6
Value : 2 3 7 10 11 11 25

Condition:
arr[i] > 11

F F F F F F T
```

Notice:

```text
False False False True True True
```

The condition changes only once.

Whenever we see

```text
FFFFTTTT
```

or

```text
TTTTFFFF
```

Binary Search is the natural solution.

Here we need the **first True**.

---

## CORE IDEA:

Maintain an answer.

Whenever

```cpp
arr[mid] > target
```

this index can be the upper bound.

But maybe an even smaller valid index exists.

So:

- Store answer.
- Move left.

Otherwise,

```cpp
arr[mid] <= target
```

Upper bound cannot be here.

Move right.

---

## BRUTE FORCE:

### Idea

Traverse from left to right.

Return the first index where

```cpp
arr[i] > target
```

If never found,

return `n`.

### Code

```cpp
int upperBound(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] > target)
            return i;
    }
    return arr.size();
}
```

### Time Complexity

**O(n)**

### Space Complexity

**O(1)**

---

## OPTIMAL APPROACH:

Use Binary Search.

Maintain

```cpp
ans = n
```

Whenever

```cpp
arr[mid] > target
```

```cpp
ans = mid;
high = mid - 1;
```

Otherwise

```cpp
low = mid + 1;
```

Eventually,

`ans` stores the first index greater than target.

---

## ALGORITHM:

```text
ans = n

low = 0
high = n - 1

while(low <= high)

    mid = low + (high - low)/2

    if(arr[mid] > target)

         ans = mid
         high = mid - 1

    else

         low = mid + 1

return ans
```

---

## DRY RUN:

### Example 1

```text
arr = [2,3,7,10,11,11,25]
target = 11
```

Initially

```text
low = 0
high = 6
ans = 7
```

### Iteration 1

```text
mid = 3

arr[3] = 10
```

```text
10 <= 11
```

Upper bound must be on the right.

```text
low = 4
```

---

### Iteration 2

```text
low = 4
high = 6

mid = 5

arr[5] = 11
```

```text
11 <= 11
```

Again move right.

```text
low = 6
```

---

### Iteration 3

```text
low = 6
high = 6

mid = 6

arr[6] = 25
```

```text
25 > 11
```

Possible answer.

```text
ans = 6

high = 5
```

Loop ends.

Return

```text
6
```

Correct.

---

### Example 2

```text
arr = [2,3,7,10]
target = 100
```

Every element is

```text
<= 100
```

Binary search never updates answer.

```text
ans = n = 4
```

Return

```text
4
```

Exactly what the problem asks.

---

## IMPORTANT CODE SNIPPETS:

### Initialize answer

```cpp
int ans = arr.size();
```

---

### Valid upper bound found

```cpp
if (arr[mid] > target) {
    ans = mid;
    high = mid - 1;
}
```

---

### Otherwise search right

```cpp
else {
    low = mid + 1;
}
```

---

### Safe mid calculation

```cpp
int mid = low + (high - low) / 2;
```

---

## COMMON MISTAKES:

### Mistake 1

Using

```cpp
>=
```

instead of

```cpp
>
```

Upper Bound requires **strictly greater**.

---

### Mistake 2

Returning immediately

```cpp
return mid;
```

Wrong.

Need the **first** occurrence.

Continue searching left.

---

### Mistake 3

Not initializing

```cpp
ans = n;
```

If no upper bound exists,

answer should be `n`.

---

### Mistake 4

Moving the wrong side

Wrong:

```cpp
if(arr[mid] > target)
    low = mid + 1;
```

Correct:

```cpp
high = mid - 1;
```

because we're searching for an earlier valid index.

---

### Mistake 5

Confusing Lower Bound and Upper Bound.

| Lower Bound | Upper Bound |
|-------------|-------------|
| `>= target` | `> target` |

Only one symbol changes.

---

## WHY I MIGHT FORGET THIS:

Lower Bound and Upper Bound are almost identical.

Remember:

```text
Lower Bound
>= target

Upper Bound
> target
```

Everything else remains exactly the same.

---

## INTERVIEW FLOW:

### Step 1

Array is sorted.

Need the first index satisfying a condition.

---

### Step 2

Condition is

```cpp
arr[i] > target
```

This creates

```text
False False False True True
```

Binary Search works.

---

### Step 3

Whenever

```cpp
arr[mid] > target
```

- Store answer.
- Search left.

Otherwise,

Search right.

---

### Step 4

If answer never changes,

return

```cpp
n
```

---

## TIME COMPLEXITY:

Each iteration halves the search space.

```text
n
n/2
n/4
n/8
...
```

Number of halvings:

```text
log₂ n
```

Therefore,

**Time Complexity = O(log n)**

---

## SPACE COMPLEXITY:

Only

- low
- high
- mid
- ans

are used.

**Space Complexity = O(1)**

---

## EDGE CASES:

### Single Element

```text
[5]

target = 2

Answer = 0
```

---

### Target Smaller Than All Elements

```text
[5,7,9]

target = 1

Answer = 0
```

---

### Target Larger Than All Elements

```text
[2,3,5]

target = 10

Answer = 3
```

---

### Duplicates

```text
[2,2,2,2,5]

target = 2

Answer = 4
```

---

### All Equal

```text
[7,7,7]

target = 7

Answer = 3
```

---

## PATTERN RECOGNITION:

Use this Binary Search whenever you hear:

- First element greater than X
- Upper Bound
- First index satisfying a condition
- Smallest index where value becomes larger
- Find insertion position after duplicates

Or whenever the condition forms:

```text
False False False True True
```

Think:

> **Binary Search for the First True.**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int upperBound(vector<int>& arr, int target) {
        int n = arr.size();

        int low = 0;
        int high = n - 1;
        int ans = n;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] > target) {
                ans = mid;          // Possible upper bound
                high = mid - 1;     // Search for an earlier one
            } else {
                low = mid + 1;      // Upper bound must be on the right
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Initialize answer

```cpp
int ans = n;
```

Assume no upper bound exists.

If we never find an element greater than `target`, returning `n` is correct.

---

### Compute middle safely

```cpp
int mid = low + (high - low) / 2;
```

Avoids integer overflow.

---

### Check condition

```cpp
if (arr[mid] > target)
```

We found a valid upper bound candidate.

---

### Save answer

```cpp
ans = mid;
```

Store this valid index.

---

### Search left

```cpp
high = mid - 1;
```

Maybe there's an earlier valid index.

---

### Search right

```cpp
low = mid + 1;
```

`arr[mid] <= target`, so this index and everything left of it cannot be the answer.

---

### Return answer

```cpp
return ans;
```

Returns the first index with `arr[index] > target`, or `n` if no such element exists.

---

# Easy-to-Remember Summary

- **Upper Bound = First element > target.**
- Pattern: **First True Binary Search (`FFFFTTTT`)**.
- If `arr[mid] > target`:
  - Save `mid`.
  - Move left (`high = mid - 1`).
- Else:
  - Move right (`low = mid + 1`).
- Initialize `ans = n` to handle the "not found" case automatically.
- The only difference from Lower Bound is the comparison:

| Lower Bound | Upper Bound |
|-------------|-------------|
| `arr[mid] >= target` | `arr[mid] > target` |
