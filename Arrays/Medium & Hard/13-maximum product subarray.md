
## PROBLEM:
Given an array containing positive, negative, and zero values, find the **maximum product of any contiguous subarray**.

Unlike Maximum Sum Subarray, multiplication behaves differently because:
- Negative × Negative = Positive
- Multiplication by 0 resets the product
- A very small (negative) product can later become the largest positive product

---

## PATTERN:
**Kadane's Algorithm Variation (Track Maximum & Minimum Product)**

---

## WHY THIS PATTERN:

For Maximum Sum (Kadane), we only keep the **maximum sum ending at the current index** because a negative running sum will never help future sums.

For products, this logic fails.

Example:

Current products ending here:

Maximum = 5

Minimum = -20

Next element = -3

5 × (-3) = -15

-20 × (-3) = 60

The previous **minimum** suddenly becomes the **maximum**.

Therefore, we must store both:
- Maximum product ending here
- Minimum product ending here

---

## CORE IDEA:

At every index there are only two choices:

1. Start a new subarray from the current element.
2. Extend the previous subarray.

To correctly extend a previous subarray we must know:

- Maximum product ending at the previous index
- Minimum product ending at the previous index

If the current element is negative:

Maximum ↔ Minimum

They swap roles because multiplication by a negative flips the sign.

---

# BRUTE FORCE:

### Idea

Generate every possible subarray.

Maintain a running product while extending the subarray.

Update the answer after every extension.

### Code

```cpp
int maxProduct(vector<int>& arr)
{
    int n = arr.size();

    int ans = INT_MIN;

    for(int i = 0; i < n; i++)
    {
        int product = 1;

        for(int j = i; j < n; j++)
        {
            product *= arr[j];

            ans = max(ans, product);
        }
    }

    return ans;
}
```

### Dry Run

Array

```
[2,-3,4]
```

Start

```
i = 0
product = 1
```

```
j = 0

product = 1 × 2 = 2

ans = 2
```

```
j = 1

product = 2 × (-3)

= -6

ans = 2
```

```
j = 2

product = -6 × 4

= -24

ans = 2
```

Now

```
i = 1

product = 1
```

```
j = 1

product = -3

ans = 2
```

```
j = 2

product = -12

ans = 2
```

Now

```
i = 2

product = 4

ans = 4
```

Final Answer

```
4
```

### Time Complexity

```
O(N²)
```

### Space Complexity

```
O(1)
```

---

# OPTIMAL APPROACH:

Instead of checking every starting index,

store only two values while traversing the array.

```
Maximum product ending here

Minimum product ending here
```

Why minimum?

Because

```
(-20) × (-5)

=100
```

The worst product may become the best product after another negative.

---

# ALGORITHM:

Initialize

```cpp
maxProd = arr[0];
minProd = arr[0];
ans = arr[0];
```

Traverse from index 1.

For every element,

### Step 1

If current element is negative,

```cpp
swap(maxProd, minProd);
```

Reason:

Negative multiplication flips signs.

---

### Step 2

Update maximum.

```cpp
maxProd = max(arr[i], arr[i] * maxProd);
```

Either

- Start new subarray
- Extend previous maximum

---

### Step 3

Update minimum.

```cpp
minProd = min(arr[i], arr[i] * minProd);
```

Either

- Start new subarray
- Extend previous minimum

---

### Step 4

Update answer.

```cpp
ans = max(ans, maxProd);
```

Repeat until the end.

---

# DRY RUN:

Array

```
[-2,6,-3,-10,0,2]
```

### Initial State

```
maxProd = -2
minProd = -2
ans = -2
```

Reason:

Only one subarray exists.

```
[-2]
```

---

## Current = 6

Positive

No swap.

Update maximum

```
max(6,6×-2)

=6
```

Meaning

Two choices

```
Start new

[6]

Product = 6
```

OR

```
Extend

[-2,6]

Product = -12
```

Choose

```
6
```

Update minimum

```
min(6,6×-2)

=-12
```

Store this because another negative may convert it into the maximum.

Update answer

```
max(-2,6)

=6
```

Current State

| maxProd | minProd | ans |
|---------:|---------:|----:|
| 6 | -12 | 6 |

---

## Current = -3

Negative

Swap first.

Before

```
maxProd = 6

minProd = -12
```

After

```
maxProd = -12

minProd = 6
```

Why?

Because multiplying by a negative flips the signs.

Update maximum

```
max(-3,-3×-12)

=36
```

Choices

```
Start new

[-3]

=-3
```

OR

```
Extend

[-2,6,-3]

36
```

Choose

```
36
```

Update minimum

```
min(-3,-3×6)

=-18
```

Update answer

```
max(6,36)

36
```

Current State

| maxProd | minProd | ans |
|---------:|---------:|----:|
| 36 | -18 | 36 |

---

## Current = -10

Negative

Swap.

Before

```
36

-18
```

After

```
-18

36
```

Update maximum

```
max(-10,-10×-18)

180
```

Update minimum

```
min(-10,-10×36)

-360
```

Update answer

```
180
```

Current State

| maxProd | minProd | ans |
|---------:|---------:|----:|
| 180 | -360 | 180 |

---

## Current = 0

Positive

No swap.

Update maximum

```
max(0,0)

=0
```

Update minimum

```
min(0,0)

=0
```

Notice:

Zero automatically resets both values.

No special handling is required.

Answer remains

```
180
```

Current State

| maxProd | minProd | ans |
|---------:|---------:|----:|
| 0 | 0 | 180 |

---

## Current = 2

Update maximum

```
max(2,2×0)

2
```

Update minimum

```
min(2,2×0)

0
```

Update answer

```
180
```

Final Answer

```
180
```

---

# IMPORTANT CODE SNIPPETS:

### Swap on negative

```cpp
if(arr[i] < 0)
    swap(maxProd, minProd);
```

---

### Update maximum

```cpp
maxProd = max(arr[i], arr[i] * maxProd);
```

---

### Update minimum

```cpp
minProd = min(arr[i], arr[i] * minProd);
```

---

### Update answer

```cpp
ans = max(ans, maxProd);
```

---

# COMMON MISTAKES:

### 1. Using Kadane directly

Maximum Sum logic does not work for products.

---

### 2. Tracking only maximum

Fails for

```
[2,-3,-4]
```

Need minimum too.

---

### 3. Forgetting the swap

Negative numbers flip signs.

Without swapping, the updates become incorrect.

---

### 4. Initializing with 1

Wrong for

```
[-2]
```

Always initialize using

```cpp
arr[0]
```

---

### 5. Handling zero separately

Not required.

The formulas automatically reset the products to zero.

---

# WHY I MIGHT FORGET THIS:

Because it looks almost identical to Kadane.

Difference:

Maximum Sum

```
Negative sum is useless.

Discard it.
```

Maximum Product

```
Negative product may become maximum later.

Never discard it.

Store both maximum and minimum.
```

---

# INTERVIEW FLOW:

1. Explain brute force.
2. Mention that products are reused.
3. Ask whether Kadane can be applied.
4. Show why storing only the maximum fails.
5. Use

```
[2,-3,-4]
```

to prove it.

6. Derive the need for both maximum and minimum.
7. Explain why negatives flip signs.
8. Introduce swap.
9. Write recurrence.
10. Analyze complexity.

---

# TIME COMPLEXITY:

```
O(N)
```

Reason:

Single traversal.

Every iteration performs constant work.

---

# SPACE COMPLEXITY:

```
O(1)
```

Reason:

Only three variables are maintained.

```
maxProd

minProd

ans
```

---

# EDGE CASES:

Single element

```
[-5]
```

Answer

```
-5
```

---

All positives

```
[2,3,4]
```

Answer

```
24
```

---

Even number of negatives

```
[-2,-3,-4,-5]
```

Whole array.

---

Odd number of negatives

```
[-2,-3,-4]
```

Algorithm automatically skips one negative.

---

Contains zero

```
[-2,0,-3]
```

Zero resets products.

---

All zeros

```
[0,0,0]
```

Answer

```
0
```

---

Starts with zero

```
[0,-2,-3]
```

Answer

```
6
```

---

# PATTERN RECOGNITION:

Whenever you see

- Maximum Product
- Negative numbers
- Contiguous subarray
- Sign flipping
- "Ending at current index"

Think

> **Kadane Variant → Track Maximum Product Ending Here + Minimum Product Ending Here.**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int maxProduct(vector<int>& arr) {

        int maxProd = arr[0];
        int minProd = arr[0];
        int ans = arr[0];

        for(int i = 1; i < arr.size(); i++)
        {
            // Negative flips signs
            if(arr[i] < 0)
                swap(maxProd, minProd);

            // Either start a new subarray
            // or extend the previous maximum
            maxProd = max(arr[i], arr[i] * maxProd);

            // Either start a new subarray
            // or extend the previous minimum
            minProd = min(arr[i], arr[i] * minProd);

            // Update global answer
            ans = max(ans, maxProd);
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int maxProd = arr[0];
```

Stores the largest product of any subarray ending at the previous index.

---

```cpp
int minProd = arr[0];
```

Stores the smallest product ending at the previous index because it may become the largest after another negative.

---

```cpp
if(arr[i] < 0)
    swap(maxProd, minProd);
```

A negative number flips the sign.

Previous maximum becomes candidate minimum.

Previous minimum becomes candidate maximum.

---

```cpp
maxProd = max(arr[i], arr[i] * maxProd);
```

Either

- Start a new subarray.
- Extend the previous maximum.

Choose whichever gives the larger product.

---

```cpp
minProd = min(arr[i], arr[i] * minProd);
```

Similarly maintain the smallest product because it may become the largest later.

---

```cpp
ans = max(ans, maxProd);
```

The best product ending at the current index is a candidate for the final answer.

---

# Easy-to-Remember Summary

### Maximum Sum

- Keep one state.
- Negative sum is useless.
- Reset when sum becomes negative.

### Maximum Product

- Keep two states.
- Negative product is valuable.
- Store maximum and minimum.
- Swap on negatives.
- Update both.

## Memory Trick

> **"Sum keeps one bucket. Product keeps two buckets because negatives can turn the worst into the best."**
