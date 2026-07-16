# Number of Occurrences in Sorted Array

## PROBLEM:

Given a **sorted array** `arr[]` and a `target`, find **how many times the target appears** in the array.

### Example

```text
arr = [1,1,2,2,2,2,3]
target = 2

Answer = 4
```

---

## PATTERN:

**Binary Search on Boundaries (First Occurrence + Last Occurrence)**

---

## WHY THIS PATTERN:

Since the array is **sorted**, all occurrences of a target are **contiguous**.

Instead of checking every element, find:

- First occurrence of target
- Last occurrence of target

Then,

```text
Occurrences = Last Index - First Index + 1
```

Each boundary can be found using Binary Search.

---

## CORE IDEA:

Think of the target as one continuous block.

```text
1 1 2 2 2 2 3
    ^       ^
 First     Last
```

Once both boundaries are known,

```text
Count = Last - First + 1
```

---

## BRUTE FORCE:

### Idea

Traverse the entire array and count every occurrence.

### Code

```cpp
int countFreq(vector<int>& arr, int target) {
    int count = 0;

    for (int x : arr) {
        if (x == target)
            count++;
    }

    return count;
}
```

### Dry Run

```text
arr = [1,1,2,2,2,2,3]

count = 0

1 -> no
1 -> no
2 -> 1
2 -> 2
2 -> 3
2 -> 4
3 -> no

Answer = 4
```

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

Perform Binary Search **twice**.

### Step 1

Find the **First Occurrence**

Whenever target is found:

- Save index
- Continue searching left

### Step 2

Find the **Last Occurrence**

Whenever target is found:

- Save index
- Continue searching right

Finally,

```text
Count = Last - First + 1
```

If target is absent,

```text
Return 0
```

---

# ALGORITHM:

## First Occurrence

```text
low = 0
high = n-1

while(low <= high)

    mid

    if(arr[mid] == target)

        ans = mid
        high = mid - 1

    else if(arr[mid] < target)

        low = mid + 1

    else

        high = mid - 1
```

---

## Last Occurrence

```text
low = 0
high = n-1

while(low <= high)

    mid

    if(arr[mid] == target)

        ans = mid
        low = mid + 1

    else if(arr[mid] < target)

        low = mid + 1

    else

        high = mid - 1
```

---

## Final Answer

```text
if(first == -1)

    return 0

return last - first + 1
```

---

# DRY RUN:

## Example

```text
arr = [1,1,2,2,2,2,3]
target = 2
```

---

### First Occurrence

Initial

```text
low = 0
high = 6
```

Iteration 1

```text
mid = 3

arr[3] = 2

ans = 3

Move Left

high = 2
```

Iteration 2

```text
low = 0
high = 2

mid = 1

arr[1] = 1

Move Right

low = 2
```

Iteration 3

```text
low = 2
high = 2

mid = 2

arr[2] = 2

ans = 2

Move Left

high = 1
```

Stop

```text
First = 2
```

---

### Last Occurrence

Initial

```text
low = 0
high = 6
```

Iteration 1

```text
mid = 3

arr[3] = 2

ans = 3

Move Right

low = 4
```

Iteration 2

```text
low = 4
high = 6

mid = 5

arr[5] = 2

ans = 5

Move Right

low = 6
```

Iteration 3

```text
low = 6
high = 6

mid = 6

arr[6] = 3

Move Left

high = 5
```

Stop

```text
Last = 5
```

Final Answer

```text
Count = 5 - 2 + 1

= 4
```

---

# IMPORTANT CODE SNIPPETS:

## First Occurrence

```cpp
if(arr[mid] == target)
{
    ans = mid;
    high = mid - 1;
}
```

---

## Last Occurrence

```cpp
if(arr[mid] == target)
{
    ans = mid;
    low = mid + 1;
}
```

---

## Count

```cpp
if(first == -1)
    return 0;

return last - first + 1;
```

---

# COMMON MISTAKES:

### Mistake 1

Stopping Binary Search immediately after finding target.

Wrong.

Need to continue searching for the boundary.

---

### Mistake 2

Returning

```text
last - first
```

Correct

```text
last - first + 1
```

---

### Mistake 3

Not checking

```text
first == -1
```

Without this,

```text
(-1) - (-1) + 1 = 1
```

Wrong.

Return

```text
0
```

---

### Mistake 4

Using normal Binary Search.

Normal Binary Search only guarantees **one occurrence**, not the first or last.

---

# WHY I MIGHT FORGET THIS:

The instinct after finding the target is to stop searching.

Don't.

Remember:

> **The goal is not to find the target.**
>
> **The goal is to find the boundary.**

First Occurrence

```text
Found Target

↓

Move Left
```

Last Occurrence

```text
Found Target

↓

Move Right
```

---

# INTERVIEW FLOW:

> Since the array is sorted, a linear scan works in O(n), but we can do better.
>
> All occurrences of the target form one continuous block.
>
> I'll perform Binary Search twice:
>
> - once to find the first occurrence,
> - once to find the last occurrence.
>
> If the target is absent, return 0.
>
> Otherwise,
>
> Count = Last − First + 1.

---

# TIME COMPLEXITY:

Two Binary Searches

Each Binary Search takes

```text
O(log n)
```

Overall

```text
O(log n) + O(log n)

= O(log n)
```

Reason:

Constant factors are ignored.

---

# SPACE COMPLEXITY:

Only variables are used.

```text
O(1)
```

---

# EDGE CASES:

### Target Not Present

```text
[1,2,3,4]

target = 5

Answer = 0
```

---

### Single Occurrence

```text
[1,2,3]

target = 2

Answer = 1
```

---

### Entire Array Is Target

```text
[2,2,2,2]

Answer = 4
```

---

### Single Element

```text
[5]

target = 5

Answer = 1
```

---

### Single Element (Absent)

```text
[5]

target = 2

Answer = 0
```

---

### Target At Beginning

```text
[2,2,3,4]
```

---

### Target At End

```text
[1,2,3,4,4]
```

---

# PATTERN RECOGNITION:

Use this pattern whenever you see:

- Sorted Array
- Duplicate values
- Count occurrences
- Frequency of a value
- First & Last Position
- Range of equal elements

### Trigger Sentence

> **"Sorted array + duplicates + find count/range of a target."**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int firstOccurrence(vector<int>& arr, int target) {
        int low = 0, high = arr.size() - 1;
        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                ans = mid;
                high = mid - 1;
            }
            else if (arr[mid] < target) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    int lastOccurrence(vector<int>& arr, int target) {
        int low = 0, high = arr.size() - 1;
        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                ans = mid;
                low = mid + 1;
            }
            else if (arr[mid] < target) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return ans;
    }

    int countFreq(vector<int>& arr, int target) {
        int first = firstOccurrence(arr, target);

        if (first == -1)
            return 0;

        int last = lastOccurrence(arr, target);

        return last - first + 1;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `int ans = -1;`

Assume target is absent.

If we never find it, `-1` is returned.

---

### `int mid = low + (high - low) / 2;`

Overflow-safe way of finding the middle index.

---

### `if(arr[mid] == target)`

We found an occurrence.

But it may **not** be the boundary.

---

### `ans = mid;`

Store the current valid answer.

Even if a better boundary exists, we don't lose this one.

---

### `high = mid - 1;`

Used while finding the **First Occurrence**.

Search further left.

---

### `low = mid + 1;`

Used while finding the **Last Occurrence**.

Search further right.

---

### `if(first == -1)`

If first occurrence doesn't exist,

the target doesn't exist.

Return

```text
0
```

---

### `return last - first + 1;`

Number of elements between two indices (inclusive).

---

# EASY-TO-REMEMBER SUMMARY

- Binary Search on **Boundaries**
- Find **First Occurrence**
- Find **Last Occurrence**
- First → Move Left
- Last → Move Right
- Count = `Last - First + 1`
- If First = -1 → Return 0
- Time = **O(log n)**
- Space = **O(1)**

### One-line Memory Trick

> **Find Left Boundary + Find Right Boundary = Size of the Target Block**
