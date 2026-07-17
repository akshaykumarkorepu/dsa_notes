

# PROBLEM:

Given a **sorted array of distinct elements** that has been **rotated at an unknown pivot**, find the index of a given target.

Example:

```
Original : 1 2 3 4 5 6 7 8 9

Rotated  : 5 6 7 8 9 1 2 3 4
```

Find the target in **O(log n)** time.

---

# PATTERN:

**Modified Binary Search (Binary Search on Rotated Sorted Array)**

---

# WHY THIS PATTERN:

A normal Binary Search only works when the **entire array is sorted**.

Here, the array is rotated, so the whole array isn't sorted.

However, one important property still holds:

> **At every iteration, one half of the array is always sorted.**

This allows us to eliminate half of the search space every iteration, just like Binary Search.

---

# CORE IDEA:

Every iteration, ask only **3 questions**.

### 1. Did I find the target?

```cpp
if(arr[mid] == key)
    return mid;
```

If yes → Return.

---

### 2. Which half is sorted?

```cpp
if(arr[low] <= arr[mid])
```

If true

```
low -------- mid
```

is sorted.

Otherwise

```
mid -------- high
```

is sorted.

---

### 3. Can the target lie inside the sorted half?

If yes

→ Search that half.

If no

→ Search the opposite half.

---

# BRUTE FORCE:

### Idea

Traverse the array linearly.

```cpp
for(int i=0;i<n;i++){
    if(arr[i]==key)
        return i;
}
return -1;
```

### Dry Run

```
5 6 7 8 1 2 3

key = 2

5 → No
6 → No
7 → No
8 → No
1 → No
2 → Found
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

Instead of finding the rotation point first,

directly perform Binary Search.

At every iteration:

1. Find the middle.
2. Identify the sorted half.
3. Check if the target belongs to that sorted half.
4. Search accordingly.

---

# ALGORITHM:

### Step 1

```cpp
low = 0;
high = n-1;
```

---

### Step 2

Repeat while

```cpp
low <= high
```

---

### Step 3

Find middle.

```cpp
mid = low + (high-low)/2;
```

---

### Step 4

Check

```cpp
if(arr[mid] == key)
```

If true

Return answer.

---

### Step 5

Check

```cpp
if(arr[low] <= arr[mid])
```

If true

Left half is sorted.

Now ask

```cpp
arr[low] <= key && key < arr[mid]
```

If true

```cpp
high = mid-1;
```

Else

```cpp
low = mid+1;
```

---

If false

Left half contains the rotation point.

Therefore,

Right half must be sorted.

Now check

```cpp
arr[mid] < key && key <= arr[high]
```

If true

```cpp
low = mid+1;
```

Else

```cpp
high = mid-1;
```

---

Repeat until

```cpp
low > high
```

Return

```cpp
-1
```

---

# DRY RUN:

## Example 1 (Left Half Sorted)

```
arr = [5,6,7,8,9,10,1,2,3]

key = 2
```

### Iteration 1

```
L               H

5 6 7 8 9 10 1 2 3
        ↑
       mid
```

```
mid = 4

arr[mid] = 9
```

Not target.

Check

```cpp
arr[low] <= arr[mid]
```

```
5 <= 9

True
```

So

```
5 6 7 8 9
```

is sorted.

Now ask

Can

```
2
```

lie inside

```
5...9 ?
```

Check

```cpp
5 <= 2 && 2 < 9
```

False.

So discard left half.

```cpp
low = mid+1;
```

New search space

```
10 1 2 3
```

---

### Iteration 2

```
10 1 2 3
```

```
mid = 6

arr[mid]=1
```

Not target.

Check

```cpp
10 <= 1
```

False.

Left isn't sorted.

Therefore,

Right half

```
1 2 3
```

must be sorted.

Now check

```cpp
1 < 2 && 2 <= 3
```

True.

Search right.

```cpp
low = mid+1;
```

---

### Iteration 3

```
2 3
```

```
mid = 7

arr[mid]=2
```

Found.

Return

```
7
```

---

## Example 2 (Left Half NOT Sorted)

Current search window

```
10 1 2 3
```

```
L      H

10 1 2 3
   ↑
  mid
```

Check

```cpp
arr[low] <= arr[mid]
```

```
10 <= 1

False
```

So

```
10 1
```

contains the rotation point.

Since a rotated sorted array has **only one rotation point**, the right half cannot contain another break.

Therefore

```
1 2 3
```

must be sorted.

Suppose

```
key = 10
```

Check

```cpp
1 < 10 && 10 <= 3
```

False.

Target cannot lie inside

```
1 2 3
```

Search left.

```cpp
high = mid-1;
```

Only

```
10
```

remains.

Found.

---

# IMPORTANT OBSERVATIONS:

### Observation 1

A rotated sorted array has exactly **one rotation point (one drop).**

Example

```
5 6 7 8 9 1 2 3

          ↓

Only

9 → 1
```

breaks the order.

---

### Observation 2

Because there is only one rotation point,

**exactly one half is always sorted.**

---

### Observation 3

If

```cpp
arr[low] <= arr[mid]
```

Left half is sorted.

Otherwise,

Left half contains the rotation point,

so the right half must be sorted.

---

### Observation 4

Never search both halves.

Binary Search always eliminates one half.

---

# IMPORTANT CODE SNIPPETS:

### Find Mid

```cpp
int mid = low + (high-low)/2;
```

---

### Check Left Sorted

```cpp
if(arr[low] <= arr[mid])
```

---

### Target Inside Left Half

```cpp
if(arr[low] <= key && key < arr[mid])
```

---

### Search Left

```cpp
high = mid-1;
```

---

### Search Right

```cpp
low = mid+1;
```

---

### Target Inside Right Half

```cpp
if(arr[mid] < key && key <= arr[high])
```

---

# COMMON MISTAKES:

1. Forgetting to check

```cpp
if(arr[mid]==key)
```

first.

2. Thinking both halves are unsorted.

Impossible.

Only one half can contain the rotation point.

3. Using incorrect comparisons.

Correct

Left

```cpp
arr[low] <= key && key < arr[mid]
```

Right

```cpp
arr[mid] < key && key <= arr[high]
```

4. Searching both halves.

Binary Search never searches both halves.

5. Using

```cpp
(high+low)/2
```

instead of

```cpp
low+(high-low)/2
```

---

# WHY I MIGHT FORGET THIS:

Because people try to memorize four different cases.

Instead remember only these three questions:

1. Did I find the target?
2. Which half is sorted?
3. Can the target lie inside the sorted half?

The code naturally follows.

---

# INTERVIEW FLOW:

> Since the array is rotated, normal Binary Search cannot be directly applied because the entire array is not sorted. However, a rotated sorted array has exactly one rotation point, so at every iteration one half of the current search space is always sorted. I first identify the sorted half using `arr[low] <= arr[mid]`. Then I check whether the target lies within the range of that sorted half. If it does, I continue searching there; otherwise, I discard it and search the opposite half. Since one half is eliminated in every iteration, the time complexity remains O(log n).

---

# TIME COMPLEXITY:

### O(log n)

Reason

Every iteration removes half of the search space.

```
n

↓

n/2

↓

n/4

↓

...

↓

1
```

Number of iterations

```
log₂(n)
```

---

# SPACE COMPLEXITY:

### O(1)

Reason

Only three pointers are used.

```
low

high

mid
```

---

# EDGE CASES:

- Single element.
- Array not rotated.
- Target at first index.
- Target at last index.
- Target equals pivot element.
- Target absent.
- Rotation by zero positions.

---

# PATTERN RECOGNITION:

Use this whenever you see:

- Rotated Sorted Array
- Distinct Elements
- Need O(log n)
- Search for a target
- Unknown Pivot

Mental Trigger:

> **Rotated Sorted Array + Search + O(log n) → Modified Binary Search**

Remember only these 3 questions:

1. Did I find the target?
2. Which half is sorted?
3. Can the target lie inside the sorted half?

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int search(vector<int>& arr, int key) {

        int low = 0;
        int high = arr.size() - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            // Target found
            if (arr[mid] == key)
                return mid;

            // Left half is sorted
            if (arr[low] <= arr[mid]) {

                // Target lies in left half
                if (arr[low] <= key && key < arr[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            }

            // Right half is sorted
            else {

                // Target lies in right half
                if (arr[mid] < key && key <= arr[high])
                    low = mid + 1;
                else
                    high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `low = 0, high = n-1`

Start with the entire array.

---

### `while(low <= high)`

Continue while the search space exists.

---

### `mid = low + (high-low)/2`

Split the search space into two halves safely.

---

### `if(arr[mid] == key)`

Always check the easiest possibility first.

---

### `if(arr[low] <= arr[mid])`

Ask:

**Is the left half sorted?**

If yes,

the rotation point is not inside the left half.

---

### `if(arr[low] <= key && key < arr[mid])`

Ask:

**Can the target lie inside the sorted left half?**

If yes

Search left.

Otherwise

Search right.

---

### `else`

Left isn't sorted.

Therefore,

the left half contains the rotation point.

Since there is only one rotation point,

the right half must be sorted.

---

### `if(arr[mid] < key && key <= arr[high])`

Ask:

**Can the target lie inside the sorted right half?**

If yes

Search right.

Otherwise

Search left.

---

### `return -1`

Target doesn't exist.

---

# EASY-TO-REMEMBER SUMMARY

## Three Questions

1. Did I find the target?

2. Which half is sorted?

3. Can the target lie inside the sorted half?

## Golden Fact

> A rotated sorted array has **exactly one rotation point (one break in increasing order).**

Therefore,

**one half is always sorted.**

Identify that sorted half.

If the target belongs there,

search it.

Otherwise,

search the other half.
