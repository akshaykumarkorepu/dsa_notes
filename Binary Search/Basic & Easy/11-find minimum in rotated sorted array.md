
# PROBLEM:

Given a **sorted array of distinct elements** that has been **rotated** at some unknown index, find the **minimum element** in **O(log N)** time.

Example:

```text
Input : [5,6,7,1,2,3,4]
Output: 1
```

---

# PATTERN:

**Binary Search on Rotated Sorted Array**

---

# WHY THIS PATTERN:

Normally Binary Search works only on a completely sorted array.

Here, although the entire array is not sorted, it still has a very useful property:

- It consists of **two individually sorted halves**.
- The minimum element is exactly where these two halves meet (rotation point).
- In every iteration, we can identify **which half contains the minimum** and discard the other half.

Since we eliminate **half of the search space** every iteration, Binary Search gives **O(log N)** complexity.

---

# CORE IDEA:

Instead of searching for the minimum directly,

**ask one question every iteration:**

> **"Which half does `arr[mid]` belong to?"**

Visualize the array as:

```text
5 6 7 | 1 2 3 4
```

Notice an important property.

### Left Half

```text
5 > 4
6 > 4
7 > 4
```

Every element is **greater than `arr[high]`**.

### Right Half

```text
1 <= 4
2 <= 4
3 <= 4
4 <= 4
```

Every element is **less than or equal to `arr[high]`**.

So simply compare

```cpp
arr[mid]
```

with

```cpp
arr[high]
```

There are three possible cases.

---

## Case 1 : arr[mid] > arr[high]

Example

```text
5 6 7 | 1 2 3 4
    ↑
   mid
```

```
arr[mid] = 7
arr[high] = 4

7 > 4
```

This tells us

- mid belongs to the **left half**
- Minimum cannot be in the left half
- Discard the left half including mid

```cpp
low = mid + 1;
```

---

## Case 2 : arr[mid] < arr[high]

Example

```text
5 6 7 | 1 2 3 4
      ↑
     mid
```

```
arr[mid] = 1
arr[high] = 4

1 < 4
```

This tells us

- mid belongs to the **right half**
- Minimum could be:
    - mid itself
    - somewhere left of mid

So we cannot remove mid.

```cpp
high = mid;
```

Notice

**NOT**

```cpp
high = mid - 1;
```

because mid itself may be the answer.

---

## Case 3 : arr[mid] == arr[high]

### For THIS problem

This problem says

> **Distinct Elements**

Therefore,

inside the loop,

this case **never occurs**.

Why?

Because every element is unique.

The only time

```text
arr[mid] == arr[high]
```

can happen is when

```text
low == high
```

But our loop condition is

```cpp
while(low < high)
```

so the loop already stops.

Therefore, inside the loop only these two situations exist:

```cpp
arr[mid] > arr[high]
```

or

```cpp
arr[mid] < arr[high]
```

That is why we simply write

```cpp
if(arr[mid] > arr[high])
    low = mid + 1;
else
    high = mid;
```

The `else` effectively means

```cpp
arr[mid] < arr[high]
```

for distinct arrays.

---

## If Duplicates Were Allowed (LeetCode 154)

Example

```text
2 2 2 0 1 2
      ↑   ↑
     mid high
```

```
arr[mid] = 2
arr[high] = 2
```

Now

```text
arr[mid] == arr[high]
```

Can we determine which half contains the minimum?

No.

Both halves contain the same value.

Binary Search loses information.

So we shrink the search space by removing one duplicate.

```cpp
if(arr[mid] > arr[high])
    low = mid + 1;

else if(arr[mid] < arr[high])
    high = mid;

else
    high--;
```

This duplicate case **does NOT apply** to the current problem.

---

# BRUTE FORCE:

## Idea

Traverse the entire array while maintaining the smallest element.

### Code

```cpp
int findMin(vector<int>& arr){

    int mini = arr[0];

    for(int x : arr)
        mini = min(mini, x);

    return mini;
}
```

### Dry Run

```text
5 6 1 2 3

mini = 5

6 -> no

1 -> update

2 -> no

3 -> no

Answer = 1
```

### Time Complexity

```
O(N)
```

### Space Complexity

```
O(1)
```

---

# OPTIMAL APPROACH:

Instead of searching for the minimum directly,

identify whether mid belongs to the **left half** or the **right half**.

Compare

```cpp
arr[mid]
```

with

```cpp
arr[high]
```

- Greater → Go Right
- Smaller → Keep Mid and Go Left

Repeat until only one index remains.

---

# ALGORITHM:

```
low = 0
high = n-1

while(low < high)

    mid = low + (high-low)/2

    if(arr[mid] > arr[high])

        low = mid + 1

    else

        high = mid

return arr[low]
```

---

# DRY RUN:

Example

```text
5 6 7 1 2 3 4
```

### Initial

```text
Index

0 1 2 3 4 5 6

Array

5 6 7 1 2 3 4
L           H
```

```
low = 0
high = 6
```

---

### Iteration 1

```
mid = 3
```

```text
5 6 7 1 2 3 4
L     M       H
```

```
arr[mid] = 1
arr[high] = 4

1 < 4
```

mid belongs to the right half.

Minimum may be mid.

```
high = mid
```

Now

```
low = 0
high = 3
```

Search space

```text
5 6 7 1
```

---

### Iteration 2

```
mid = 1
```

```text
5 6 7 1
L M   H
```

```
arr[mid] = 6
arr[high] = 1

6 > 1
```

mid belongs to the left half.

Minimum cannot be there.

```
low = mid + 1
```

Now

```
low = 2
high = 3
```

Search space

```text
7 1
```

---

### Iteration 3

```
mid = 2
```

```text
7 1
M H
```

```
7 > 1
```

```
low = mid + 1
```

Now

```
low = high = 3
```

Loop ends.

Return

```cpp
arr[3]
```

Answer

```
1
```

---

# IMPORTANT OBSERVATIONS:

- Rotated array consists of two sorted halves.
- Every element in the left half is **greater than arr[high]**.
- Every element in the right half is **less than or equal to arr[high]**.
- We are NOT searching for the minimum directly.
- We are identifying which half `mid` belongs to.

---

# IMPORTANT CODE SNIPPETS:

### Binary Search Loop

```cpp
while(low < high)
```

### Why `<` and NOT `<=` ?

We stop when

```text
low == high
```

At that point,

only **one candidate index** remains.

That must be the minimum.

If we write

```cpp
while(low <= high)
```

then when

```text
low == high
```

the loop still executes.

Example

```
low = high = 3
```

```
mid = 3
```

Now

```cpp
high = mid;
```

doesn't move anything.

Or

```cpp
low = mid + 1;
```

moves outside the valid answer.

The convergence logic becomes messy.

Using

```cpp
while(low < high)
```

means

> Continue until only one possible answer remains.

This is the cleanest Binary Search template for finding one index.

---

### Find Middle

```cpp
int mid = low + (high-low)/2;
```

---

### Left Half

```cpp
if(arr[mid] > arr[high])
    low = mid + 1;
```

---

### Right Half

```cpp
else
    high = mid;
```

---

### Return Answer

```cpp
return arr[low];
```

---

# COMMON MISTAKES:

### Mistake 1

```cpp
high = mid - 1;
```

Wrong.

mid itself may be the minimum.

---

### Mistake 2

```cpp
low = mid;
```

instead of

```cpp
low = mid + 1;
```

May cause an infinite loop.

---

### Mistake 3

Comparing

```cpp
arr[mid]
```

with

```cpp
arr[low]
```

instead of

```cpp
arr[high]
```

The comparison with `arr[high]` immediately tells us which half mid belongs to.

---

### Mistake 4

Thinking we are searching for the minimum.

Actually we are searching for the **rotation point**.

---

### Mistake 5

Assuming equality can happen in this problem.

It cannot because all elements are distinct.

---

# WHY I MIGHT FORGET THIS:

Remember only this picture.

```text
5 6 7 | 1 2 3 4
```

Left Half

```
> arr[high]
```

Right Half

```
<= arr[high]
```

Everything else follows naturally.

---

# INTERVIEW FLOW:

> "Although the array is rotated, it still consists of two sorted halves."

> "I compare arr[mid] with arr[high]."

> "If arr[mid] is greater than arr[high], then mid belongs to the left half, so the minimum must lie on the right."

> "Otherwise, mid belongs to the right half. Since mid itself may be the minimum, I keep it by moving high to mid."

> "I continue until low and high converge to one index."

---

# TIME COMPLEXITY:

## Time

```
O(log N)
```

### Reason

Every iteration removes half of the search space.

```
N

N/2

N/4

N/8
```

Hence logarithmic complexity.

---

# SPACE COMPLEXITY:

```
O(1)
```

Only constant extra variables are used.

---

# EDGE CASES:

### Single Element

```text
[5]

Answer = 5
```

---

### Already Sorted

```text
1 2 3 4 5

Answer = 1
```

---

### Rotated Once

```text
5 1 2 3 4

Answer = 1
```

---

### Rotation Near End

```text
2 3 4 5 1

Answer = 1
```

---

### Two Elements

```text
2 1

Answer = 1
```

---

### Duplicate Version (Different Problem)

```text
2 2 2 0 1 2
```

Need

```cpp
else
    high--;
```

because equality gives no information.

---

# PATTERN RECOGNITION:

Use this pattern when

- Array was originally sorted.
- Array has been rotated.
- Asked to find minimum / rotation index.
- Expected complexity is O(log N).
- One comparison with a boundary allows you to eliminate half of the search space.

Ask yourself:

> **"Can one comparison tell me which sorted half my middle element belongs to?"**

If yes,

it's a **Rotated Binary Search** problem.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int findMin(vector<int>& arr) {

        int low = 0;
        int high = arr.size() - 1;

        while (low < high) {

            int mid = low + (high - low) / 2;

            if (arr[mid] > arr[high])
                low = mid + 1;
            else
                high = mid;
        }

        return arr[low];
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int low = 0, high = arr.size() - 1;
```

Search over the complete array.

---

```cpp
while(low < high)
```

Continue until only one candidate index remains.

---

```cpp
int mid = low + (high-low)/2;
```

Find the middle safely without overflow.

---

```cpp
if(arr[mid] > arr[high])
```

mid belongs to the left half.

---

```cpp
low = mid + 1;
```

Discard the left half because the minimum cannot be there.

---

```cpp
high = mid;
```

Keep mid because it may itself be the minimum.

---

```cpp
return arr[low];
```

When low equals high, we have found the minimum.

---

# EASY-TO-REMEMBER SUMMARY

Visualize every rotated array like this.

```text
5 6 7 | 1 2 3 4
```

### Left Half

```
Values > arr[high]
```

### Right Half

```
Values <= arr[high]
```

Remember only these rules:

```cpp
if(arr[mid] > arr[high])
    low = mid + 1;     // Go Right

else
    high = mid;        // Keep Mid, Go Left
```

### Memory Trick

> **Greater than High → Go Right**

> **Less than High → Keep Mid and Go Left**

For the duplicate version:

```cpp
if(arr[mid] > arr[high])
    low = mid + 1;

else if(arr[mid] < arr[high])
    high = mid;

else
    high--;
```

Finally remember the Binary Search template:

```cpp
while(low < high)
```

because

> **We are shrinking the search space until exactly ONE candidate index remains.**
