
## PROBLEM:

Given a **sorted array of distinct integers** and a target `x`, return the **index of `x`** if it exists. Otherwise, return **`-1`**.

### Example

```text
arr = [2,4,6,8,10,12]
x = 8

Output: 3
```

---

## PATTERN:

**Binary Search**

---

## WHY THIS PATTERN:

Binary Search works whenever:

- The data is **sorted**.
- We need to **find an element**.
- We can **eliminate half of the search space** after every comparison.

Instead of checking every element one by one, Binary Search repeatedly divides the search range into half.

---

## CORE IDEA:

At every step:

- Find the middle element.
- If `mid == target` → Answer found.
- If `target < arr[mid]` → Search left half.
- If `target > arr[mid]` → Search right half.

**Every comparison removes half of the remaining elements.**

---

## BRUTE FORCE:

### Idea

Traverse the array from left to right until the target is found.

### Algorithm

```text
for every element
    if element == target
        return index

return -1
```

### Dry Run

```text
arr = [2,4,6,8,10]
x = 8

Check 2 ❌
Check 4 ❌
Check 6 ❌
Check 8 ✅

Answer = 3
```

### C++ Code

```cpp
int search(vector<int>& arr, int x) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == x)
            return i;
    }
    return -1;
}
```

### Time Complexity

**O(n)**

Reason: In the worst case, we scan the entire array.

### Space Complexity

**O(1)**

---

## OPTIMAL APPROACH:

Use **Binary Search**.

Since the array is sorted:

- Left half contains smaller values.
- Right half contains larger values.

So after every comparison, we can discard one entire half.

---

## ALGORITHM:

```text
low = 0
high = n - 1

while(low <= high)

    mid = low + (high - low) / 2

    if(arr[mid] == target)
        return mid

    else if(target < arr[mid])
        high = mid - 1

    else
        low = mid + 1

return -1
```

---

## DRY RUN:

```text
arr = [2,4,6,8,10,12]
target = 10
```

### Step 1

```text
low = 0
high = 5

mid = 2

arr[mid] = 6

10 > 6

Search Right Half

low = 3
```

### Step 2

```text
low = 3
high = 5

mid = 4

arr[mid] = 10

Found

Answer = 4
```

---

## IMPORTANT CODE SNIPPETS:

### Safe Middle Calculation

```cpp
int mid = low + (high - low) / 2;
```

Avoids integer overflow.

---

### Move Left

```cpp
high = mid - 1;
```

---

### Move Right

```cpp
low = mid + 1;
```

---

### Loop Condition

```cpp
while (low <= high)
```

Search continues while at least one element is left.

---

## COMMON MISTAKES:

### ❌ Using

```cpp
while(low < high)
```

May skip the last remaining element.

---

### ❌ Unsafe Middle Calculation

```cpp
mid = (low + high) / 2;
```

Use instead:

```cpp
mid = low + (high - low) / 2;
```

Reason: Prevents integer overflow.

---

### ❌ Wrong Updates

Wrong:

```cpp
low = mid;
```

Correct:

```cpp
low = mid + 1;
```

Otherwise, the search space may never shrink.

Similarly,

Wrong:

```cpp
high = mid;
```

Correct:

```cpp
high = mid - 1;
```

---

### ❌ Forgetting

```cpp
return -1;
```

when the target is absent.

---

## WHY I MIGHT FORGET THIS:

- Forget whether to write `<` or `<=`
- Write `low = mid` instead of `mid + 1`
- Write `high = mid` instead of `mid - 1`
- Use `(low + high) / 2`
- Forget that every comparison removes half of the search space

---

## INTERVIEW FLOW:

### Step 1

"The array is already sorted."

### Step 2

"A linear search would take O(n), but since the array is sorted, we can do better."

### Step 3

"I'll use Binary Search."

### Step 4

"I compare the middle element with the target."

- Equal → Return answer
- Smaller → Search right half
- Larger → Search left half

### Step 5

"Each comparison removes half of the remaining search space."

### Step 6

"So the overall complexity becomes O(log n)."

---

## TIME COMPLEXITY:

### O(log n)

### Reason

Each iteration removes half of the remaining search space.

Example:

```text
16

↓

8

↓

4

↓

2

↓

1
```

Only **5 comparisons**.

General:

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

↓

1
```

Number of divisions = **log₂(n)**

Therefore,

**Time = O(log n)**

---

## SPACE COMPLEXITY:

### O(1)

Reason:

Only three variables are used.

```text
low
high
mid
```

No extra data structure is required.

---

## EDGE CASES:

### Empty Array

```text
[]

Return -1
```

---

### Single Element

```text
[5]

Target = 5

Return 0
```

---

### Target at First Index

```text
[2,4,6]

Target = 2
```

---

### Target at Last Index

```text
[2,4,6]

Target = 6
```

---

### Target Not Present

```text
[2,4,6]

Target = 5

Return -1
```

---

### Negative Numbers

```text
[-10,-5,0,4]
```

Binary Search still works because the array remains sorted.

---

## PATTERN RECOGNITION:

Think **Binary Search** when you see:

- ✅ Sorted array
- ✅ Search for an element
- ✅ First / Last occurrence
- ✅ Lower Bound / Upper Bound
- ✅ Insert Position
- ✅ Peak Element
- ✅ Rotated Sorted Array
- ✅ Binary Search on Answer
- ✅ Monotonic search space

Ask yourself:

> **"Can I eliminate half of the search space after one comparison?"**

If **Yes**, think **Binary Search**.

---

# Clean C++ Code

```cpp
class Solution {
public:
    int binarySearch(vector<int>& arr, int x) {
        int low = 0;
        int high = arr.size() - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == x)
                return mid;

            if (arr[mid] < x)
                low = mid + 1;
            else
                high = mid - 1;
        }

        return -1;
    }
};
```

---

# Intuition Behind Every Important Line

### `int low = 0;`

Start searching from the first index.

---

### `int high = arr.size() - 1;`

End searching at the last index.

---

### `while (low <= high)`

Continue while there is a valid search space.

---

### `int mid = low + (high - low) / 2;`

Find the middle element safely without integer overflow.

---

### `if (arr[mid] == x) return mid;`

The middle element is the target.

Return its index immediately.

---

### `if (arr[mid] < x)`

Target is larger than the middle element.

So it must lie in the right half.

---

### `low = mid + 1;`

Discard the left half (including `mid`) and search the right half.

---

### `else`

Target is smaller than the middle element.

---

### `high = mid - 1;`

Discard the right half (including `mid`) and search the left half.

---

### `return -1;`

The search space is exhausted.

The target does not exist in the array.

---

# Easy-to-Remember Summary

- **Sorted Array → Think Binary Search**
- Maintain two pointers: `low` and `high`.
- Compute:

```cpp
mid = low + (high - low) / 2;
```

- If `arr[mid] == target` → Return `mid`.
- If `arr[mid] < target` → Search right (`low = mid + 1`).
- Otherwise → Search left (`high = mid - 1`).
- Stop when `low > high`.
- **Golden Rule:** Every comparison removes half of the search space, giving **O(log n)** time.
