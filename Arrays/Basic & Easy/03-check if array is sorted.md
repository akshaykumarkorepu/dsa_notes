

## PROBLEM:
Given an array `arr[]`, determine whether it is sorted in **non-decreasing order**.

**Non-decreasing** means:

```text
arr[i] <= arr[i+1]
```

Equal elements are allowed.

**Examples:**

```text
[1, 2, 2, 3, 5]   → Sorted ✅
[1, 2, 5, 4, 6]   → Not Sorted ❌
```

---

## PATTERN:
**Single Linear Traversal (Adjacent Comparison)**

---

## WHY THIS PATTERN:

A sorted array has one simple property:

> Every element should be **less than or equal to the next element**.

Instead of checking the whole array at once, we only need to verify this property for every adjacent pair.

If even one adjacent pair violates this rule, the array is not sorted.

---

## CORE IDEA:

Traverse from left to right.

For every index:

```cpp
if(arr[i] > arr[i+1])
    return false;
```

If no violation is found after checking all adjacent pairs, return `true`.

---

## BRUTE FORCE:

### Is there a meaningful brute force?

**No.**

The optimal solution is already straightforward.

Approaches like:

- Sorting the array and comparing it with the original
- Comparing every pair of elements

are inefficient and not expected in interviews.

Interviewers directly expect the linear scan.

---

## OPTIMAL APPROACH:

Traverse the array once.

Compare every element with its next element.

- If `arr[i] > arr[i+1]`, return `false`.
- Otherwise continue.

If the loop finishes, return `true`.

---

## ALGORITHM:

```text
For i = 0 to n-2

    If arr[i] > arr[i+1]
        return false

Return true
```

---

## DRY RUN:

### Example 1

```text
arr = [10, 20, 30, 40, 50]
```

```
10 <= 20 ✅
20 <= 30 ✅
30 <= 40 ✅
40 <= 50 ✅
```

No violation found.

**Answer:** `true`

---

### Example 2

```text
arr = [90, 80, 100, 70]
```

```
90 > 80 ❌
```

Violation found immediately.

Return `false`.

No need to check the remaining elements.

This is called **Early Exit**.

---

### Example 3

```text
arr = [1, 2, 2, 2, 5]
```

```
1 <= 2 ✅
2 <= 2 ✅
2 <= 2 ✅
2 <= 5 ✅
```

Equal values are allowed.

**Answer:** `true`

---

## IMPORTANT CODE SNIPPETS:

### Adjacent Comparison

```cpp
if(arr[i] > arr[i+1])
    return false;
```

---

### Correct Loop

```cpp
for(int i = 0; i < n-1; i++)
```

We stop at `n-2` because we compare `arr[i]` with `arr[i+1]`.

---

### Final Return

```cpp
return true;
```

Reached only if no violation exists.

---

## COMMON MISTAKES:

### Mistake 1: Using `>=`

Wrong:

```cpp
if(arr[i] >= arr[i+1])
```

This incorrectly treats duplicate values as unsorted.

Example:

```text
[1,2,2,3]
```

is sorted.

Correct:

```cpp
if(arr[i] > arr[i+1])
```

---

### Mistake 2: Wrong Loop Boundary

Wrong:

```cpp
for(int i=0;i<n;i++)
```

This accesses:

```cpp
arr[i+1]
```

which causes an out-of-bounds error.

Correct:

```cpp
for(int i=0;i<n-1;i++)
```

---

### Mistake 3: Comparing Every Pair

```text
arr[i] <= arr[j]
```

Not required.

Only adjacent elements matter.

---

### Mistake 4: Sorting the Array First

```cpp
sort(arr.begin(), arr.end());
```

This changes the original array and wastes time.

---

## WHY I MIGHT FORGET THIS:

The problem looks so easy that people often overthink it.

Remember:

> **A sorted array is completely defined by its adjacent pairs.**

If every adjacent pair is correct, the whole array is sorted.

---

## INTERVIEW FLOW:

> "A non-decreasing array satisfies `arr[i] <= arr[i+1]` for every adjacent pair. I'll perform one linear traversal and compare each element with the next one. If I find any index where `arr[i] > arr[i+1]`, I'll immediately return false because the sorted property is violated. Otherwise, after checking all adjacent pairs, I'll return true. This takes O(n) time and O(1) extra space."

---

## TIME COMPLEXITY:

### **O(n)**

### Reason:

- Maximum comparisons = `n-1`
- Each adjacent pair is checked exactly once.

Worst Case:

```text
Array is completely sorted.
```

Need to check every element.

Best Case:

```text
First comparison fails.
```

Return immediately.

---

## SPACE COMPLEXITY:

### **O(1)**

Reason:

Only one loop variable is used.

No extra data structure is required.

---

## EDGE CASES:

### Single Element

```text
[5]
```

Sorted.

---

### Two Elements

```text
[2,3] → true
[3,2] → false
```

---

### Duplicate Values

```text
[2,2,2,2]
```

Sorted.

---

### Negative Numbers

```text
[-10,-5,0,4]
```

Sorted.

---

### Large Values

```text
[-1e9, 1e9]
```

Still works because only comparisons are performed.

---

### Empty Array

If allowed, it is considered sorted.

(Problem guarantees at least one element.)

---

## PATTERN RECOGNITION:

Use this pattern whenever you see:

- Check if array is sorted
- Verify ordering
- Validate increasing sequence
- Detect first violation
- Check monotonic array
- Is sequence non-decreasing?
- Verify ordering constraints

### Trigger Sentence:

> **"I only need to verify a property between adjacent elements while scanning once."**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    bool arraySortedOrNot(vector<int>& arr) {
        int n = arr.size();

        for(int i = 0; i < n - 1; i++) {
            if(arr[i] > arr[i + 1])
                return false;
        }

        return true;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int n = arr.size();
```

Store the size once to avoid repeated function calls.

---

```cpp
for(int i = 0; i < n - 1; i++)
```

Iterate until the second-last element because we'll compare `arr[i]` with `arr[i+1]`.

---

```cpp
if(arr[i] > arr[i + 1])
```

If the current element is greater than the next, the sorted property is violated.

---

```cpp
return false;
```

One violation is enough to conclude the array is not sorted.

---

```cpp
return true;
```

Executed only if every adjacent pair satisfies the sorted property.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Linear Scan + Adjacent Comparison
- **Key Property:** `arr[i] <= arr[i+1]` for every adjacent pair
- **Loop:** `for(i = 0; i < n-1; i++)`
- **Condition:** `if(arr[i] > arr[i+1]) return false;`
- **Otherwise:** `return true`
- **Time Complexity:** **O(n)** (Best Case: **O(1)** due to early exit)
- **Space Complexity:** **O(1)**

### One-Line Interview Summary

> **Scan the array once and verify every adjacent pair satisfies `arr[i] <= arr[i+1]`. The first violation proves the array is not sorted; otherwise, it is sorted.**
````
