
# Move All Zeroes to End

## PROBLEM

Given an array `arr[]`, move all the `0`s to the end **while maintaining the relative order of the non-zero elements**.

**Conditions:**
- Perform the operation **in-place**
- Do **not** use another array for the optimal solution.

### Example

```text
Input : [1,2,0,4,3,0,5,0]
Output: [1,2,4,3,5,0,0,0]
```

---

# PATTERN

**Two Pointers (Read Pointer + Write Pointer / Stable Compaction)**

---

# WHY THIS PATTERN

This problem requires us to:

- Traverse the array once.
- Keep the order of non-zero elements unchanged.
- Do everything in-place.

This naturally leads to the **Read Pointer + Write Pointer** pattern.

- **Read Pointer (`i`)** → Reads every element.
- **Write Pointer (`j`)** → Marks where the next non-zero element should be placed.

---

# CORE IDEA

Don't think:

> **Move all zeros.**

Think:

> **Collect all non-zero elements at the front.**

The zeros automatically shift to the end.

Example:

```text
Original

1 2 0 4 3 0 5 0

↓

Collect all non-zero values

1 2 4 3 5 _ _ _

↓

Remaining positions naturally become zero

1 2 4 3 5 0 0 0
```

---

# BRUTE FORCE

## Idea

Create another vector.

1. Copy every non-zero element.
2. Append zeros.
3. Copy everything back into the original array.

---

## Code

```cpp
void pushZerosToEnd(vector<int>& arr){

    vector<int> temp;

    for(int i=0;i<arr.size();i++){

        if(arr[i]!=0){

            temp.push_back(arr[i]);

        }
    }

    while(temp.size()<arr.size()){

        temp.push_back(0);

    }

    arr=temp;
}
```

---

## Dry Run

Input

```text
1 2 0 4 3 0 5 0
```

Create

```text
temp = []
```

Read elements one by one

```text
Read 1

temp = [1]
```

↓

```text
Read 2

temp = [1,2]
```

↓

```text
Read 0

Ignore
```

↓

```text
Read 4

temp = [1,2,4]
```

↓

```text
Read 3

temp = [1,2,4,3]
```

↓

```text
Read 0

Ignore
```

↓

```text
Read 5

temp = [1,2,4,3,5]
```

↓

```text
Read 0

Ignore
```

Append remaining zeros

```text
temp

1 2 4 3 5 0 0 0
```

Copy back

```text
arr

1 2 4 3 5 0 0 0
```

---

## Time Complexity

- First loop → **O(n)**
- Append zeros → **O(n)** (worst case)
- Copy back → **O(n)**

Overall

```text
O(n)
```

---

## Space Complexity

Extra vector stores up to `n` elements.

```text
O(n)
```

---

# OPTIMAL APPROACH

## Intuition

In the brute-force solution, we created another vector only to remember where non-zero elements should go.

Instead,

**use the original array itself as that temporary vector.**

Maintain two pointers.

- `i` → Reads every element.
- `j` → Always points to the **next position where a non-zero should be placed.**

Whenever `i` finds a non-zero,

swap it with `arr[j]`

then increment `j`.

---

# ALGORITHM

1. Initialize

```cpp
int j = 0;
```

2. Traverse the array.

3. If current element is non-zero,

```cpp
swap(arr[i], arr[j]);
j++;
```

4. Continue until traversal finishes.

---

# DRY RUN

## Code

```cpp
int j = 0;

for(int i=0;i<arr.size();i++){

    if(arr[i]!=0){

        swap(arr[i],arr[j]);

        j++;

    }
}
```

---

## Initial State

```text
Array

1 2 0 4 3 0 5 0

i = 0
j = 0
```

### Meaning of `j`

> `j` always points to the **next position where a non-zero element should be placed.**

---

## Iteration 1

Current state

```text
Array

1 2 0 4 3 0 5 0

i = 0
j = 0
```

Current element

```cpp
arr[i]
```

↓

```text
arr[0] = 1
```

Condition

```cpp
if(arr[i]!=0)
```

↓

True

Swap

```cpp
swap(arr[0],arr[0]);
```

Array

```text
1 2 0 4 3 0 5 0
```

No change.

Increment

```cpp
j++;
```

Now

```text
j = 1
```

Meaning

> Index 0 is correct.
>
> The next non-zero belongs at index 1.

---

## Iteration 2

Current state

```text
Array

1 2 0 4 3 0 5 0

i = 1
j = 1
```

Current element

```text
2
```

Condition

True

Swap

```cpp
swap(arr[1],arr[1]);
```

Array

```text
1 2 0 4 3 0 5 0
```

Increment

```text
j = 2
```

Meaning

The first two non-zero elements are correctly placed.

---

## Iteration 3

Current state

```text
Array

1 2 0 4 3 0 5 0

i = 2
j = 2
```

Current element

```text
0
```

Condition

False

So compiler skips

```cpp
swap(...)
```

and

```cpp
j++;
```

Nothing happens.

Current state

```text
Array

1 2 0 4 3 0 5 0

i = 3
j = 2
```

Notice

`j` did **not** move because index 2 is still waiting for a non-zero.

---

## Iteration 4

Current state

```text
Array

1 2 0 4 3 0 5 0

i = 3
j = 2
```

Current element

```text
4
```

Condition

True

Swap

```cpp
swap(arr[3],arr[2]);
```

Before

```text
1 2 0 4 3 0 5 0
```

After

```text
1 2 4 0 3 0 5 0
```

Increment

```cpp
j++;
```

Now

```text
j = 3
```

Meaning

The first three non-zero elements are correctly placed.

---

## Iteration 5

Current state

```text
Array

1 2 4 0 3 0 5 0

i = 4
j = 3
```

Current element

```text
3
```

Swap

```cpp
swap(arr[4],arr[3]);
```

After

```text
1 2 4 3 0 0 5 0
```

Increment

```text
j = 4
```

---

## Iteration 6

Current state

```text
Array

1 2 4 3 0 0 5 0

i = 5
j = 4
```

Current element

```text
0
```

Condition fails.

Nothing happens.

`j` remains 4.

---

## Iteration 7

Current state

```text
Array

1 2 4 3 0 0 5 0

i = 6
j = 4
```

Current element

```text
5
```

Swap

```cpp
swap(arr[6],arr[4]);
```

After

```text
1 2 4 3 5 0 0 0
```

Increment

```text
j = 5
```

---

## Iteration 8

Current element

```text
0
```

Condition fails.

Loop ends.

Final array

```text
1 2 4 3 5 0 0 0
```

---

# IMPORTANT OBSERVATION

The most important idea is:

> **`j` does NOT point to a zero.**

It points to

> **the next position where a non-zero element should be placed.**

At every moment,

Everything before `j`

```text
0 ........ j-1
```

already contains all non-zero elements encountered so far in the correct order.

---

# IMPORTANT CODE SNIPPETS

### Initialize write pointer

```cpp
int j = 0;
```

---

### Traverse array

```cpp
for(int i=0;i<arr.size();i++)
```

---

### Ignore zeros

```cpp
if(arr[i]!=0)
```

---

### Move current non-zero

```cpp
swap(arr[i],arr[j]);
j++;
```

---

# COMMON MISTAKES

❌ Thinking `j` points to a zero.

Correct:

`j` points to the next position where a non-zero should be placed.

---

❌ Incrementing `j` when current element is zero.

Increment only after placing a non-zero.

---

❌ Using sorting.

Sorting destroys the relative order.

---

❌ Using another array in the optimal solution.

---

# WHY I MIGHT FORGET THIS

Because I think

> Move zeros.

Instead remember

> Place every non-zero into its next correct position.

---

# INTERVIEW FLOW

> We need to move all zeros to the end while maintaining the order of non-zero elements and doing it in-place.

> A brute-force solution is to create another vector, copy all non-zero elements, append zeros, and copy everything back. This takes O(n) extra space.

> To optimize, I use two pointers.

> `i` scans every element.

> `j` always points to the next position where a non-zero should be placed.

> Whenever I encounter a non-zero element, I swap it with `arr[j]` and increment `j`.

> This guarantees that everything before `j` always contains all non-zero elements encountered so far in their correct order.

---

# TIME COMPLEXITY

One traversal.

Each element is visited once.

Each swap takes O(1).

```text
Time = O(n)
```

---

# SPACE COMPLEXITY

Only two indices are used.

```text
Space = O(1)
```

---

# EDGE CASES

### No zeros

```text
1 2 3

↓

1 2 3
```

---

### All zeros

```text
0 0 0

↓

0 0 0
```

---

### Single element

```text
0

or

5
```

---

### Zero at beginning

```text
0 1 2

↓

1 2 0
```

---

### Consecutive zeros

```text
1 0 0 2

↓

1 2 0 0
```

---

# PATTERN RECOGNITION

Use this pattern whenever you see:

- Move all X to one side.
- Preserve relative order.
- In-place modification.
- Compact valid elements.
- Stable rearrangement.

Typical questions:

- Move Zeroes
- Remove Element
- Remove Duplicates from Sorted Array
- Stable Partition
- Compact Array

---

# Clean C++ Code

```cpp
class Solution {
public:

    void pushZerosToEnd(vector<int>& arr) {

        int j = 0;

        for(int i = 0; i < arr.size(); i++) {

            if(arr[i] != 0) {

                swap(arr[i], arr[j]);

                j++;

            }
        }
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int j = 0;
```

`j` always points to the next position where a non-zero should be placed.

---

```cpp
for(int i=0;i<arr.size();i++)
```

`i` reads every element.

---

```cpp
if(arr[i]!=0)
```

Ignore zeros.

---

```cpp
swap(arr[i],arr[j]);
```

Move the current non-zero to its correct position.

The zero automatically shifts backward.

---

```cpp
j++;
```

One more non-zero has been correctly placed.

Move to the next available position.

---

# EASY TO REMEMBER SUMMARY

✅ Pattern → **Two Pointers (Read + Write)**

✅ Think

> **Don't move zeros. Collect non-zero elements.**

- `i` → Reads every element.
- `j` → Next position where a non-zero belongs.
- If `arr[i] != 0`
    - `swap(arr[i], arr[j])`
    - `j++`

**Interview Trigger**

> "Move elements while preserving order and doing it in-place."

**Time Complexity:** `O(n)`

**Space Complexity:** `O(1)`
````
