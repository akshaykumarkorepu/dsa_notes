

## PROBLEM:

Given an array containing both positive and negative numbers:

- Rearrange it into **alternate positive and negative numbers**.
- The answer **must start with a positive number** (`0` is also considered positive).
- **Maintain the relative order** of positive numbers.
- **Maintain the relative order** of negative numbers.
- If one type of element gets exhausted, append the remaining elements while maintaining their order.

---

# PATTERN:

**Stable Partition + Merge (Two Auxiliary Arrays + Three Pointers)**

---

# WHY THIS PATTERN:

The biggest clue in the problem is:

> **Maintain the relative order**

Whenever you see:

- Maintain relative order
- Stable ordering
- Preserve original order

You should immediately think:

> **Separate the elements into groups first, then merge them.**

Using swaps destroys the original order, so this pattern naturally fits.

---

# CORE IDEA:

Imagine making **two separate queues**.

Input

```text
9 4 -2 -1 5 0 -5 -3 2
```

Separate them.

Positive

```text
9 4 5 0 2
```

Negative

```text
-2 -1 -5 -3
```

Now merge them alternately.

```text
Positive
Negative
Positive
Negative
...
```

If one queue finishes,

append the remaining elements.

---

# BRUTE FORCE:

## Important Note

### **For this question, Brute Force and Optimal are the SAME.**

Normally we have

```text
Brute → Better → Optimal
```

But here,

because the problem asks us to **maintain relative order**, there is no meaningful simpler approach using swaps.

The natural solution of

- Separating positives
- Separating negatives
- Merging them

is already the best practical solution.

So in interviews you can say:

> "Due to the stability (relative order) requirement, the straightforward auxiliary-array solution is also the optimal solution."

### Approach

**Step 1**

Store all positives.

**Step 2**

Store all negatives.

**Step 3**

Fill the original array alternately.

**Step 4**

Append whichever group remains.

### Time Complexity

```
O(N)
```

### Space Complexity

```
O(N)
```

---

# OPTIMAL APPROACH:

Exactly the same as the brute force.

Why?

Because:

- We must preserve order.
- Swapping destroys order.
- A linear-time in-place stable rearrangement is not straightforward.
- Using two vectors is already the intended and optimal solution.

---

# ALGORITHM:

### Step 1

Create two vectors.

```cpp
vector<int> pos;
vector<int> neg;
```

---

### Step 2

Traverse the array once.

If element ≥ 0

store in positive vector.

Else

store in negative vector.

Example

```text
Input

9 4 -2 -1 5 0 -5 -3 2

↓

Positive

9 4 5 0 2

Negative

-2 -1 -5 -3
```

---

### Step 3

Create three pointers.

```text
p -> positive vector

n -> negative vector

i -> original array
```

---

### Step 4

While both vectors have elements,

copy

Positive

then

Negative.

---

### Step 5

If positives remain,

append all positives.

---

### Step 6

If negatives remain,

append all negatives.

Done.

---

# DRY RUN:

Input

```text
arr = [9,4,-2,-1,5,0,-5,-3,2]
```

### Separate

Positive

```text
[9,4,5,0,2]
```

Negative

```text
[-2,-1,-5,-3]
```

Pointers

```text
p=0

n=0

i=0
```

### First Iteration

Take positive

```text
arr[0]=9
```

Array

```text
9 _ _ _ _ _ _ _ _
```

Move

```text
i=1

p=1
```

Take negative

```text
arr[1]=-2
```

Array

```text
9 -2 _ _ _ _ _ _ _
```

Move

```text
i=2

n=1
```

---

### Second Iteration

Take positive

```text
4
```

Array

```text
9 -2 4 _ _ _ _ _ _
```

Take negative

```text
-1
```

Array

```text
9 -2 4 -1 _ _ _ _ _
```

---

### Third Iteration

Take

```text
5
```

Then

```text
-5
```

Array

```text
9 -2 4 -1 5 -5 _ _ _
```

---

### Fourth Iteration

Take

```text
0
```

Then

```text
-3
```

Array

```text
9 -2 4 -1 5 -5 0 -3 _
```

---

Negatives finished.

Append remaining positives.

```text
2
```

Final

```text
9 -2 4 -1 5 -5 0 -3 2
```

---

# IMPORTANT CODE SNIPPETS:

### Separate positives and negatives

```cpp
for (int x : arr) {

    if (x >= 0)
        pos.push_back(x);
    else
        neg.push_back(x);
}
```

---

### Alternate merge

```cpp
while (p < pos.size() && n < neg.size()) {

    arr[i] = pos[p];
    i++;
    p++;

    arr[i] = neg[n];
    i++;
    n++;
}
```

---

### Append remaining positives

```cpp
while (p < pos.size()) {

    arr[i] = pos[p];
    i++;
    p++;
}
```

---

### Append remaining negatives

```cpp
while (n < neg.size()) {

    arr[i] = neg[n];
    i++;
    n++;
}
```

---

# COMMON MISTAKES:

### Mistake 1

Treating

```
0
```

as negative.

Problem clearly says

```
0 is positive.
```

---

### Mistake 2

Using swaps.

Swaps destroy the original order.

---

### Mistake 3

Using

```cpp
while(p<pos.size() || n<neg.size())
```

This is wrong.

It should be

```cpp
while(p<pos.size() && n<neg.size())
```

The remaining elements are handled separately.

---

### Mistake 4

Forgetting to append remaining elements.

---

### Mistake 5

Starting with a negative.

The problem explicitly says

Start with positive.

---

# WHY I MIGHT FORGET THIS:

Because I may start thinking:

> "Can I solve it in-place?"

But the words

> **Maintain relative order**

immediately tell me

> Separate first.

Not swap.

Remember

```
Separate → Alternate → Append
```

---

# INTERVIEW FLOW:

> "The key observation is that the problem requires maintaining the relative order of both positive and negative numbers. An in-place swapping approach would break this stability. So I first separate all positive numbers (including zero) and all negative numbers into two vectors while preserving their order. Then I use three pointers—one for the positive vector, one for the negative vector, and one for the original array—to merge them alternately, always starting with a positive. Once either vector is exhausted, I append the remaining elements from the other vector. This gives an O(N) time and O(N) space solution."

---

# TIME COMPLEXITY:

### Separating positives and negatives

```
O(N)
```

### Alternate merge

Every element is copied exactly once.

```
O(N)
```

### Appending remaining elements

Worst case

```
O(N)
```

Overall

```
O(N)+O(N)+O(N)

=

O(N)
```

Reason:

Every element is visited a constant number of times.

---

# SPACE COMPLEXITY:

Extra vectors

```cpp
vector<int> pos;
vector<int> neg;
```

Together they store

```
N
```

elements.

Hence

```
O(N)
```

---

# EDGE CASES:

### Only positives

```
1 2 3
```

Output

```
1 2 3
```

---

### Only negatives

```
-1 -2 -3
```

Output

```
-1 -2 -3
```

(No positive exists to start with.)

---

### More positives

```
1 2 3 -1
```

Output

```
1 -1 2 3
```

---

### More negatives

```
-1 -2 -3 1
```

Output

```
1 -1 -2 -3
```

---

### Zeros

```
0 2 -1
```

Zero is treated as positive.

---

### Equal positives and negatives

Perfect alternation.

---

# PATTERN RECOGNITION:

Whenever you see:

- Maintain relative order
- Stable ordering
- Preserve order
- Rearrange into groups
- Alternate two categories

Think

> **Stable Partition + Merge**

Recipe

```
Separate

↓

Alternate

↓

Append Remaining
```

Never think of swapping first.

---

# Clean C++ Code

```cpp
class Solution {
public:
    void rearrange(vector<int> &arr) {

        // Store positives and negatives separately
        vector<int> pos;
        vector<int> neg;

        // Step 1: Separate positives and negatives
        for (int x : arr) {

            if (x >= 0)
                pos.push_back(x);      // Positive (0 is also positive)
            else
                neg.push_back(x);      // Negative
        }

        // p -> positive vector
        // n -> negative vector
        // i -> original array
        int p = 0;
        int n = 0;
        int i = 0;

        // Step 2: Fill alternately
        while (p < pos.size() && n < neg.size()) {

            arr[i] = pos[p];
            i++;
            p++;

            arr[i] = neg[n];
            i++;
            n++;
        }

        // Step 3: Append remaining positives
        while (p < pos.size()) {

            arr[i] = pos[p];
            i++;
            p++;
        }

        // Step 4: Append remaining negatives
        while (n < neg.size()) {

            arr[i] = neg[n];
            i++;
            n++;
        }
    }
};
```

---

# Intuition Behind Every Important Line

### Create two vectors

```cpp
vector<int> pos;
vector<int> neg;
```

→ Store positives and negatives separately while maintaining their order.

---

### Traverse the array

```cpp
for (int x : arr)
```

→ Visit every element exactly once.

---

### Separate

```cpp
if (x >= 0)
    pos.push_back(x);
else
    neg.push_back(x);
```

→ Build two stable groups.

---

### Three pointers

```cpp
int p = 0;
int n = 0;
int i = 0;
```

- `p` → positive vector
- `n` → negative vector
- `i` → original array

---

### Alternate

```cpp
arr[i] = pos[p];
i++;
p++;
```

→ Put one positive and move both pointers.

```cpp
arr[i] = neg[n];
i++;
n++;
```

→ Put one negative and move both pointers.

---

### Append remaining positives

```cpp
while (p < pos.size())
```

→ If positives are left, copy them.

---

### Append remaining negatives

```cpp
while (n < neg.size())
```

→ If negatives are left, copy them.

---

# Easy-to-Remember Summary

## Trigger

> **Alternate + Maintain Relative Order**

## Pattern

> **Stable Partition + Merge**

## Recipe

```
Separate

↓

Alternate

↓

Append Remaining
```

## Important Interview Point

> **For this question, Brute Force and Optimal are the SAME because the stability (relative order) requirement makes the auxiliary-array approach the intended and optimal solution.**

## Complexity

- **Time:** `O(N)`
- **Space:** `O(N)`
