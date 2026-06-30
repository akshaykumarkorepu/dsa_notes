
## PROBLEM:
Given an array `arr[]`, find and return the **largest element** present in the array.

**Example:**
```
Input:  arr = [1, 8, 7, 56, 90]
Output: 90
```

---

## PATTERN:
**Linear Traversal (Single Pass Scan)**

---

## WHY THIS PATTERN:

Whenever the question asks for:
- Largest element
- Smallest element
- Maximum value
- Minimum value
- Sum of elements
- Count of elements satisfying a condition

You don't need to compare every element with every other element.

Instead, **visit every element exactly once while maintaining the current answer.**

This is one of the most fundamental array patterns.

---

## CORE IDEA:

Think of it like this:

Suppose you've only seen the first few elements of the array.

Can you always know the largest among them?

Yes.

Whenever you see a new element:
- If it is larger than the current maximum, update the maximum.
- Otherwise, ignore it.

By the time you've scanned the entire array, the stored maximum is the answer.

**You never need to sort the array or compare every pair of elements.**

---

## BRUTE FORCE:

### Idea

Sort the array.

After sorting, the last element will be the largest.

### Algorithm

1. Sort the array.
2. Return the last element.

### Code

```cpp
int largest(vector<int> &arr) {
    sort(arr.begin(), arr.end());
    return arr.back();
}
```

### Dry Run

```
Input:
[5, 2, 9, 1, 8]

After Sorting:
[1, 2, 5, 8, 9]

Largest = 9
```

### Time Complexity

```
O(n log n)
```

### Space Complexity

```
O(1) (implementation-dependent; std::sort uses O(log n) recursion stack)
```

### Why is this not optimal?

Sorting rearranges the entire array even though we only need one element.

This performs unnecessary work.

---

## OPTIMAL APPROACH:

Traverse the array once while maintaining the largest element seen so far.

---

## ALGORITHM:

1. Initialize `maxi = arr[0]`.
2. Traverse the array from index `1`.
3. If `arr[i] > maxi`, update `maxi`.
4. After traversal, return `maxi`.

---

## DRY RUN:

### Example

```
arr = [1, 8, 7, 56, 90]
```

Initially

```
maxi = 1
```

### i = 1

```
Element = 8

8 > 1

maxi = 8
```

### i = 2

```
Element = 7

7 > 8 ?

No

maxi = 8
```

### i = 3

```
Element = 56

56 > 8

maxi = 56
```

### i = 4

```
Element = 90

90 > 56

maxi = 90
```

Traversal complete.

Answer:

```
90
```

---

### Another Example

```
arr = [5, 5, 5, 5]
```

Initially

```
maxi = 5
```

Every comparison

```
5 > 5 ?

No
```

Answer

```
5
```

---

## IMPORTANT CODE SNIPPETS:

### Initialize maximum

```cpp
int maxi = arr[0];
```

---

### Update maximum

```cpp
if (arr[i] > maxi)
    maxi = arr[i];
```

or

```cpp
maxi = max(maxi, arr[i]);
```

---

### Return answer

```cpp
return maxi;
```

---

## COMMON MISTAKES:

### Mistake 1

Initializing maximum as

```cpp
int maxi = 0;
```

This only works when all numbers are non-negative.

Correct approach:

```cpp
int maxi = arr[0];
```

---

### Mistake 2

Sorting the array.

Sorting is unnecessary because we only need the largest element.

---

### Mistake 3

Looping from index `0` after already assigning

```cpp
maxi = arr[0];
```

Not wrong.

Just one unnecessary comparison.

Preferred:

```cpp
for(int i = 1; i < n; i++)
```

---

### Mistake 4

Returning the last index instead of the last element after sorting.

---

## WHY I MIGHT FORGET THIS:

The solution feels too simple.

Remember this rule:

> If you only need one statistic (maximum, minimum, sum, count), **don't sort**.

Ask yourself:

**Can I compute the answer while reading the array once?**

If yes, use **Single Pass Traversal**.

---

## INTERVIEW FLOW:

"I only need the maximum element, not the entire array in sorted order.

A brute-force approach is to sort the array and return the last element, which takes **O(n log n)** time.

A better approach is to maintain the maximum element seen so far while traversing the array once.

I initialize the maximum with the first element.

For every remaining element, if it is larger than the current maximum, I update it.

After one complete traversal, the stored maximum is the answer.

This gives **O(n)** time and **O(1)** extra space."

---

## TIME COMPLEXITY:

### Brute Force

```
O(n log n)
```

Reason:

Sorting dominates the complexity.

---

### Optimal

```
O(n)
```

Reason:

Every element is visited exactly once.

---

## SPACE COMPLEXITY:

### Brute Force

```
O(1) (or O(log n) recursion stack depending on sorting implementation)
```

---

### Optimal

```
O(1)
```

Reason:

Only one extra variable (`maxi`) is used.

---

## EDGE CASES:

### Single Element

```
[10]

Answer = 10
```

---

### All Elements Equal

```
[5, 5, 5, 5]

Answer = 5
```

---

### Increasing Order

```
[1, 2, 3, 4, 5]

Answer = 5
```

---

### Decreasing Order

```
[9, 7, 5, 2, 1]

Answer = 9
```

---

### Largest at Beginning

```
[100, 2, 3, 4]

Answer = 100
```

---

### Largest at End

```
[2, 5, 7, 10]

Answer = 10
```

---

## PATTERN RECOGNITION:

Whenever you see questions asking for:

- Largest element
- Smallest element
- Maximum value
- Minimum value
- Sum of elements
- Count of elements satisfying a condition
- Maximum while traversing
- Minimum while traversing

Think immediately:

**Linear Traversal + Running Answer**

Ask yourself:

> "Can I update my answer while visiting each element exactly once?"

If yes,

➡️ **Single Pass Traversal** is the correct pattern.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int largest(vector<int> &arr) {
        int maxi = arr[0];

        for (int i = 1; i < arr.size(); i++) {
            if (arr[i] > maxi)
                maxi = arr[i];
        }

        return maxi;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Line 1

```cpp
int maxi = arr[0];
```

Start by assuming the first element is the largest.

This works even if the array contains negative numbers (general case).

---

### Line 2

```cpp
for (int i = 1; i < arr.size(); i++)
```

The first element has already been considered.

Now check every remaining element exactly once.

---

### Line 3

```cpp
if (arr[i] > maxi)
```

Ask:

"Did I find a better (larger) answer?"

---

### Line 4

```cpp
maxi = arr[i];
```

Update the current largest element.

---

### Line 5

```cpp
return maxi;
```

After scanning the complete array, `maxi` stores the largest element.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Linear Traversal (Single Pass)
- **Idea:** Keep track of the largest element seen so far.
- **Initialize:** `maxi = arr[0]`
- **For every element:** If `arr[i] > maxi`, update `maxi`.
- **Never sort** when only the maximum element is required.
- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

### Memory Trick

> **Maximum = Keep a Champion**

The current maximum is the champion.

Every new element challenges the champion.

If it is larger, it becomes the new champion.

After one complete traversal, the champion is the largest element.
