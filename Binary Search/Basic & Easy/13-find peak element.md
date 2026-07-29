# Peak Element (Binary Search)

---

# NOTE

## PROBLEM:

Given an array `arr[]` where **no two adjacent elements are equal**, return the **index of any peak element**.

A peak element is one that is greater than its adjacent elements (if they exist).

- `arr[0]` is a peak if `arr[0] > arr[1]`
- `arr[n-1]` is a peak if `arr[n-1] > arr[n-2]`
- Elements outside the array are considered **−∞**, so a peak is always guaranteed to exist.

---

## PATTERN:

**Binary Search on Answer (Binary Search on Slope / Local Maximum)**

**Trigger:**
> Whenever comparing adjacent elements tells you which half must contain the answer.

---

## WHY THIS PATTERN:

Although the array is **not sorted**, it has a hidden property.

At any index:

- If `arr[mid] < arr[mid+1]`, we are moving **uphill**.
  - A peak **must exist on the right**.
- If `arr[mid] > arr[mid+1]`, we are moving **downhill**.
  - A peak **must exist on the left (including mid)**.

This allows us to discard half of the array every iteration, making Binary Search applicable.

---

## CORE IDEA:

Think of the array as a mountain.

### Case 1: Increasing Slope

```
1 3 5 7 9
      ^
```

Since you're climbing, either:

- you'll eventually reach a peak, or
- the last element becomes the peak.

**Move Right**

---

### Case 2: Decreasing Slope

```
9 7 5 3 1
^
```

You've already crossed (or are standing on) a peak.

**Move Left (including mid)**

---

### Decision Rule

```
arr[mid] < arr[mid+1]
        ↓
     Move Right

arr[mid] > arr[mid+1]
        ↓
 Move Left (including mid)
```

---

## BRUTE FORCE:

### Idea

Check every element.

Return the first element which is greater than both neighbours.

---

### Algorithm

1. Handle first element.
2. Check every middle element.
3. Handle last element.

---

### Code

```cpp
int peakElement(vector<int> &arr) {

    int n = arr.size();

    if(n == 1) return 0;

    if(arr[0] > arr[1])
        return 0;

    for(int i = 1; i < n-1; i++){

        if(arr[i] > arr[i-1] &&
           arr[i] > arr[i+1])
            return i;
    }

    if(arr[n-1] > arr[n-2])
        return n-1;

    return -1;
}
```

---

### Dry Run

```
1 2 4 5 7 8 3

1 → No

2 → No

4 → No

5 → No

7 → No

8 → YES

Return index = 5
```

---

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(1)
```

---

### Why Optimize?

The brute force solution checks every element.

However, Binary Search reduces the search space by half every iteration, giving **O(log n)** time.

---

## OPTIMAL APPROACH:

Use Binary Search on the **slope**.

Instead of checking whether `mid` itself is a peak, compare only:

```
arr[mid]
arr[mid+1]
```

This comparison alone tells which half definitely contains a peak.

---

## ALGORITHM:

```
low = 0
high = n-1

while(low < high)

    mid = low + (high-low)/2

    if(arr[mid] < arr[mid+1])
        low = mid+1

    else
        high = mid

return low
```

---

### Why is `mid+1` always safe?

Loop condition:

```
while(low < high)
```

Since:

```
mid < high
```

Therefore:

```
mid+1 <= high
```

So `arr[mid+1]` is always within bounds.

---

### Why Move Right?

```
1 3 5 7 9 6
      ^
```

Since:

```
5 < 7
```

We are climbing.

A peak must exist on the right.

```
low = mid + 1
```

---

### Why Move Left?

```
1 3 9 6 5
     ^
```

Since:

```
9 > 6
```

We are descending.

Peak is either:

- at `mid`
- before `mid`

Therefore:

```
high = mid
```

**Never**

```
high = mid - 1
```

because `mid` itself could be the peak.

---

### Why Does Binary Search End?

Eventually:

```
low == high
```

Only one candidate remains.

That index must be a peak.

---

## ALGORITHM EXPLANATION

1. Search the whole array.
2. Find the middle.
3. Compare `arr[mid]` and `arr[mid+1]`.
4. If increasing, discard the left half.
5. If decreasing, discard the right half except `mid`.
6. Continue until only one element remains.
7. Return that index.

---

## DRY RUN:

### Example

```
arr = [1,2,4,5,7,8,3]
```

Initial

```
low = 0
high = 6
```

---

### Iteration 1

```
mid = 3

arr[mid] = 5
arr[mid+1] = 7

5 < 7
```

Increasing

```
low = 4
```

---

Now

```
low = 4
high = 6
```

---

### Iteration 2

```
mid = 5

arr[5] = 8
arr[6] = 3

8 > 3
```

Descending

```
high = 5
```

---

Now

```
low = 4
high = 5
```

---

### Iteration 3

```
mid = 4

arr[4] = 7
arr[5] = 8

7 < 8
```

Increasing

```
low = 5
```

---

Now

```
low = high = 5
```

Answer

```
Peak Index = 5
```

---

## IMPORTANT OBSERVATIONS

- A peak is always guaranteed to exist.
- We only compare `arr[mid]` and `arr[mid+1]`.
- No need to compare with both neighbours.
- Binary Search works because the slope determines which side contains a peak.
- `high = mid`, not `mid-1`, because `mid` may itself be the peak.

---

## IMPORTANT CODE SNIPPETS

### Binary Search Loop

```cpp
while(low < high)
```

---

### Safe Mid Calculation

```cpp
int mid = low + (high-low)/2;
```

---

### Increasing Slope

```cpp
if(arr[mid] < arr[mid+1])
    low = mid + 1;
```

---

### Decreasing Slope

```cpp
else
    high = mid;
```

---

### Return Answer

```cpp
return low;
```

---

## COMMON MISTAKES

### 1. Writing

```cpp
high = mid - 1;
```

Wrong.

The peak could be `mid` itself.

Always write:

```cpp
high = mid;
```

---

### 2. Using

```cpp
while(low <= high)
```

Wrong for this approach.

Correct:

```cpp
while(low < high)
```

---

### 3. Forgetting why `mid+1` is safe

It is safe because:

```
low < high
```

guarantees

```
mid < high
```

---

### 4. Checking both neighbours

Not needed.

One comparison is enough.

---

### 5. Overcomplicating edge cases

This Binary Search naturally handles:

- first element
- last element
- single element

---

## WHY I MIGHT FORGET THIS

Because this Binary Search is **not searching for a value**.

It searches based on the **direction of the slope**.

Remember:

```
Increasing

     /
    /
   /

Move Right
```

```
Decreasing

\
 \
  \

Move Left
```

Simple rule:

```
Uphill

↓

Right

Downhill

↓

Left
```

---

## INTERVIEW FLOW

**Brute Force**

> "I'll first scan every element and check whether it's greater than both neighbours. This takes O(n) time."

**Optimization**

> "Instead of checking every element, I observe that comparing `arr[mid]` with `arr[mid+1]` tells whether I'm climbing or descending."

**Binary Search**

> "If I'm climbing, a peak must exist on the right. If I'm descending, a peak lies on the left, possibly at `mid` itself. Hence, I can discard half the array every iteration."

---

## TIME COMPLEXITY

### Brute Force

```
O(n)
```

Reason:

Every element is checked once.

---

### Optimal

```
O(log n)
```

Reason:

Binary Search halves the search space in every iteration.

---

## SPACE COMPLEXITY

### Brute Force

```
O(1)
```

### Optimal

```
O(1)
```

Only a few variables are used.

---

## EDGE CASES

### Single Element

```
[5]

Answer = 0
```

---

### Peak at Beginning

```
5 3 2 1

Answer = 0
```

---

### Peak at End

```
1 2 3 5

Answer = 3
```

---

### Multiple Peaks

```
1 3 2 5 4
```

Both indices are valid answers.

---

### Strictly Increasing

```
1 2 3 4 5
```

Last element is the peak.

---

### Strictly Decreasing

```
5 4 3 2 1
```

First element is the peak.

---

## PATTERN RECOGNITION

Use this pattern when:

- The array is **not sorted**.
- Comparing neighbouring elements tells you which side contains the answer.
- The question asks for:
  - Peak Element
  - Local Maximum
  - Turning Point
  - Any valid answer
- The search space can be reduced by analysing the slope.

**Trigger Statement**

> "When comparing adjacent elements lets me eliminate half of the search space."

---

# Clean C++ Code

```cpp
class Solution {
public:
    int peakElement(vector<int> &arr) {

        int low = 0;
        int high = arr.size() - 1;

        while (low < high) {

            int mid = low + (high - low) / 2;

            // Increasing slope → Peak lies on the right
            if (arr[mid] < arr[mid + 1]) {
                low = mid + 1;
            }
            // Decreasing slope → Peak lies on the left (including mid)
            else {
                high = mid;
            }
        }

        return low;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int low = 0;
int high = arr.size() - 1;
```

Search the entire array.

---

```cpp
while(low < high)
```

Continue until only one candidate remains.
Also guarantees `mid+1` is always valid.

---

```cpp
int mid = low + (high-low)/2;
```

Calculate the middle safely without integer overflow.

---

```cpp
if(arr[mid] < arr[mid+1])
```

We're climbing.

A peak must exist on the right.

---

```cpp
low = mid + 1;
```

Discard the left half.

---

```cpp
high = mid;
```

We're descending.

The peak could be `mid`, so don't discard it.

---

```cpp
return low;
```

When `low == high`, we've found the peak.

---

# Easy-to-Remember Summary

- **Pattern:** Binary Search on Slope
- `arr[mid] < arr[mid+1]` → Move Right
- `arr[mid] > arr[mid+1]` → Move Left (including mid)
- Use `while(low < high)`
- Never write `high = mid - 1`
- Compare only `arr[mid]` and `arr[mid+1]`
- Binary Search works because the slope always points towards a peak.
