
# PROBLEM:

You are given a **sorted array** where:
- Every element appears **exactly twice**
- Only **one element appears once**

Find the element that appears only once.

### Example

```text
Input : [1,1,2,2,3,3,4,50,50,65,65]

Output : 4
```

The expected solution is **O(log N)**, so we need to use **Binary Search**.

---

# PATTERN:

**Binary Search on Index Parity (Even-Odd Pair Pattern)**

This is **not** Binary Search on values.

Instead, Binary Search is performed on a **property**:

> Before the single element, pairs start at **even indices**.
>
> After the single element, pairs start at **odd indices**.

---

# WHY THIS PATTERN:

Binary Search works whenever there is a property that changes exactly once.

Here, the changing property is the pairing pattern.

### Without a single element

```text
Index

0 1
2 3
4 5
6 7

Array

1 1
2 2
3 3
4 4
```

Notice:

Every pair starts at an **even index**.

---

Now insert a single element.

```text
Array

1 1 2 2 3 3 4 50 50 65 65

Index

0 1 2 3 4 5 6 7 8 9 10
```

Observe carefully.

### Before the single element

```text
1 1  -> starts at index 0 (Even)

2 2  -> starts at index 2 (Even)

3 3  -> starts at index 4 (Even)
```

### After the single element

```text
50 50 -> starts at index 7 (Odd)

65 65 -> starts at index 9 (Odd)
```

The single element shifts every remaining pair by one position.

So we now have

```text
Left Side

Pairs start at EVEN indices

↓

Single Element

↓

Right Side

Pairs start at ODD indices
```

Since this property changes only once, Binary Search can locate the transition.

---

# CORE IDEA:

Always compare a **complete pair**.

A complete pair always starts at an **even index**.

Therefore,

1. Find `mid`.
2. If `mid` is odd, make it even.
3. Compare

```cpp
arr[mid]
```

with

```cpp
arr[mid+1]
```

If they are equal,

the pair is correct,

so search the **right half**.

If they are different,

the pairing already broke,

so search the **left half (including mid)**.

---

# BRUTE FORCE:

## Intuition

Since the array is sorted,

duplicate elements always appear together.

Simply check every pair.

The first broken pair is the answer.

If every pair is correct,

the last element is the answer.

### Code

```cpp
class Solution {
public:
    int single(vector<int>& arr) {

        int n = arr.size();

        for(int i = 0; i < n - 1; i += 2)
        {
            if(arr[i] != arr[i + 1])
                return arr[i];
        }

        return arr[n - 1];
    }
};
```

### Dry Run

```text
arr

1 1 2 2 3 3 4 50 50 65 65
```

| i | Pair | Result |
|---|------|--------|
|0|1 1|Correct|
|2|2 2|Correct|
|4|3 3|Correct|
|6|4 50|Broken → Return 4|

### Time Complexity

**O(N)**

### Space Complexity

**O(1)**

---

# OPTIMAL APPROACH:

Instead of checking every pair,

observe the pairing pattern.

### Before the answer

```text
Even Odd

AA
BB
CC
```

### After the answer

```text
Odd Even

DD
EE
FF
```

Binary Search simply finds where this pattern changes.

---

# ALGORITHM:

### Step 1

Initialize Binary Search.

```cpp
low = 0;
high = n - 1;
```

---

### Step 2

Run Binary Search while

```cpp
low < high
```

---

### Step 3

Find the middle.

```cpp
mid = low + (high - low) / 2;
```

---

### Step 4

If `mid` is odd,

make it even.

```cpp
if(mid % 2 == 1)
    mid--;
```

### Why?

We always want `mid` to point to the **first element of a pair**.

If `mid` is odd,

it points to the **second element of a pair**.

Example:

```text
Index

0 1 2 3 4 5 6 7

Array

1 1 2 2 3 3 4 4
          ^
         mid = 5
```

If we compare

```cpp
arr[mid]
arr[mid+1]
```

we compare

```text
3

4
```

These are NOT partners.

Instead,

make

```text
mid = 4
```

Now compare

```text
3

3
```

which is the correct pair.

This is the most important line in the solution.

---

### Step 5

Compare

```cpp
arr[mid]
```

and

```cpp
arr[mid+1]
```

#### Case 1

```cpp
arr[mid] == arr[mid+1]
```

The pair is correct.

Everything till this pair is perfectly paired.

Therefore,

discard this pair and search right.

```cpp
low = mid + 2;
```

Why `+2`?

Because the whole pair has already been verified.

---

#### Case 2

```cpp
arr[mid] != arr[mid+1]
```

The pairing has already broken.

The answer is

- at `mid`
- or somewhere before `mid`

Search left.

```cpp
high = mid;
```

Do **NOT** write

```cpp
high = mid - 1;
```

because `mid` itself could be the answer.

---

### Step 6

Eventually,

```cpp
low == high
```

Only one index remains.

Return

```cpp
arr[low];
```

---

# DRY RUN:

## Input

```text
1 1 2 2 3 3 4 50 50 65 65

Index

0 1 2 3 4 5 6 7 8 9 10
```

---

## Iteration 1

```text
low = 0

high = 10
```

Find mid.

```text
mid = 5
```

Mid is odd.

Make it even.

```text
mid = 4
```

Compare

```text
arr[4] = 3

arr[5] = 3
```

Equal.

Meaning,

the pair is complete.

Everything till index 5 is perfectly paired.

Discard

```text
0 to 5
```

Move

```text
low = mid + 2

low = 6
```

---

## Iteration 2

```text
low = 6

high = 10
```

Find mid.

```text
mid = 8
```

Already even.

Compare

```text
arr[8] = 50

arr[9] = 65
```

Not equal.

The pair is broken.

Therefore,

the answer is

```text
index 8

or somewhere before it.
```

Move

```text
high = 8
```

---

## Iteration 3

```text
low = 6

high = 8
```

Find mid.

```text
mid = 7
```

Odd.

Make it even.

```text
mid = 6
```

Compare

```text
arr[6] = 4

arr[7] = 50
```

Not equal.

Again,

the pair is broken.

Move

```text
high = 6
```

Now

```text
low = 6

high = 6
```

Loop stops.

Return

```text
arr[6]

↓

4
```

---

# IMPORTANT OBSERVATIONS:

- Before the unique element, every pair starts at an **even index**.
- After the unique element, every pair starts at an **odd index**.
- We are **not searching for a value**.
- We are searching for the **first place where the pairing pattern changes**.

---

# IMPORTANT CODE SNIPPETS:

### Safe Mid

```cpp
int mid = low + (high - low) / 2;
```

### Make Mid Even

```cpp
if(mid % 2 == 1)
    mid--;
```

### Pair Correct

```cpp
if(arr[mid] == arr[mid + 1])
    low = mid + 2;
```

### Pair Broken

```cpp
else
    high = mid;
```

### Return Answer

```cpp
return arr[low];
```

---

# COMMON MISTAKES:

❌ Forgetting to make `mid` even.

❌ Writing

```cpp
high = mid - 1;
```

instead of

```cpp
high = mid;
```

❌ Writing

```cpp
low = mid + 1;
```

instead of

```cpp
low = mid + 2;
```

❌ Comparing

```cpp
arr[mid]
arr[mid-1]
```

instead of

```cpp
arr[mid]
arr[mid+1]
```

---

# WHY I MIGHT FORGET THIS:

Because it feels like we're searching for a number.

We're actually searching for

> **the first place where the pairing pattern changes.**

Remember this picture.

```text
Before Answer

Even Odd

AA
BB
CC

↓

Single Element

↓

Odd Even

DD
EE
```

---

# INTERVIEW FLOW:

"I first observe that the array is sorted and every element appears twice except one.

Before the unique element, every pair starts at an even index.

After the unique element, every pair starts at an odd index because the unique element shifts every remaining pair by one position.

This creates a monotonic property, so Binary Search can be applied.

At every iteration, I force `mid` to be even so that it always points to the first element of a pair.

If `arr[mid] == arr[mid+1]`, that pair is correct, so I search the right half.

Otherwise, the pairing has already broken, so I search the left half including `mid`.

When `low == high`, that index contains the single element."

---

# TIME COMPLEXITY:

Each iteration halves the search space.

Therefore,

**Time Complexity = O(log N)**

---

# SPACE COMPLEXITY:

Only three variables are used.

No extra memory is required.

**Space Complexity = O(1)**

---

# EDGE CASES:

### Single Element

```text
[5]

Answer = 5
```

### Unique at Beginning

```text
[1,2,2,3,3]

Answer = 1
```

### Unique at End

```text
[1,1,2,2,5]

Answer = 5
```

### Unique in Middle

```text
[1,1,2,3,3]

Answer = 2
```

---

# PATTERN RECOGNITION:

Whenever you see

- Sorted array
- Duplicate elements
- One unique element
- Expected O(log N)

ask yourself

> **Does the answer divide the array into two regions with different properties?**

If yes,

Binary Search on the property is usually the solution.

For this problem,

the changing property is

```text
Before Answer

Pairs start at EVEN indices

↓

After Answer

Pairs start at ODD indices
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int single(vector<int>& arr) {

        int low = 0;
        int high = arr.size() - 1;

        while (low < high) {

            int mid = low + (high - low) / 2;

            // Always point to the first element of a pair
            if (mid % 2 == 1)
                mid--;

            if (arr[mid] == arr[mid + 1]) {
                // Pair is complete
                low = mid + 2;
            }
            else {
                // Pair is broken
                high = mid;
            }
        }

        return arr[low];
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int low = 0;
int high = arr.size() - 1;
```

Start Binary Search on the entire array.

---

```cpp
while(low < high)
```

Continue until only one possible index remains.

---

```cpp
int mid = low + (high-low)/2;
```

Find the middle safely.

---

```cpp
if(mid % 2 == 1)
    mid--;
```

If `mid` is odd, it points to the **second element of a pair**.

Move it back so that it always points to the **first element of a pair**.

This guarantees that `arr[mid]` and `arr[mid+1]` are always one complete pair.

---

```cpp
if(arr[mid] == arr[mid+1])
```

The pair is correct.

Everything up to this pair is perfectly paired.

The answer must be on the right.

---

```cpp
low = mid + 2;
```

Skip the verified pair.

---

```cpp
high = mid;
```

The pairing breaks here.

`mid` itself may be the answer, so don't discard it.

---

```cpp
return arr[low];
```

When `low == high`, only one possible index remains.

That is the answer.

---

# EASY-TO-REMEMBER SUMMARY

### Three-Step Rule

1. Make `mid` even.
2. Compare `arr[mid]` with `arr[mid+1]`.
3. Equal → Move Right (`low = mid + 2`)
   Not Equal → Move Left (`high = mid`)

### Memory Trick

```text
Before Unique

(Even, Odd) → Pair

↓

Unique Element

↓

After Unique

(Odd, Even) → Pair
```

**The unique element shifts every remaining pair by one index. Binary Search simply finds where that shift begins.**
