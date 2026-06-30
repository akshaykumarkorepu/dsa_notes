

## PROBLEM:
We need to find the **second largest distinct element** in an array.

- Second largest **must be different** from the largest.
- If every element is the same, return **-1**.

Example:
```
[12, 35, 1, 10, 34, 1]

Largest = 35
Second Largest = 34
```

---

## PATTERN:
**Maintain Multiple Extremes (Tracking Maximums)**

---

## WHY THIS PATTERN:
Instead of sorting the array (which changes order and takes extra time), we only care about:
- Largest element
- Second largest element

So while traversing the array once, we continuously update these two values.

Whenever we discover a new maximum:
- The old maximum becomes the second maximum.

Whenever we discover a number smaller than the maximum but larger than the current second maximum:
- Update the second maximum.

This is a classic **one-pass tracking** pattern.

---

## CORE IDEA:

Maintain two variables:

```cpp
largest
secondLargest
```

Initially,

```cpp
largest = -1;
secondLargest = -1;
```

For every element:

### Case 1: New Largest Found

```cpp
secondLargest = largest;
largest = current;
```

### Case 2: Better Second Largest Found

If

```cpp
current < largest &&
current > secondLargest
```

then

```cpp
secondLargest = current;
```

The condition

```cpp
current < largest
```

ensures duplicate maximum values are **not** considered as the second largest.

---

# BRUTE FORCE:

### Idea

Sort the array.

The largest element will be at the end.

Traverse backwards until you find the first element different from the largest.

Return it.

Otherwise, return `-1`.

### Code

```cpp
int getSecondLargest(vector<int> &arr) {

    sort(arr.begin(), arr.end());

    int largest = arr.back();

    for(int i = arr.size() - 2; i >= 0; i--) {

        if(arr[i] != largest)
            return arr[i];
    }

    return -1;
}
```

### Dry Run

```
Input:
12 35 1 10 34 1

After Sorting:
1 1 10 12 34 35

Largest = 35

Move left:
34 != 35

Answer = 34
```

### Time Complexity

```
O(N log N)
```

### Space Complexity

```
O(1)
```

(ignoring sorting implementation)

### Why Optimize?

Sorting rearranges the entire array even though we only need the two largest distinct values.

A single traversal is sufficient.

---

# OPTIMAL APPROACH:

Traverse the array once while maintaining:

```cpp
largest
secondLargest
```

Update them whenever required.

No sorting.

Single traversal.

---

# ALGORITHM:

```text
largest = -1
secondLargest = -1

For every element x

    if x > largest

        secondLargest = largest
        largest = x

    else if

        x < largest
        AND
        x > secondLargest

        secondLargest = x

Return secondLargest
```

---

# DRY RUN:

### Example 1

```
Input:
12 35 1 10 34 1
```

Initially

```
largest = -1
secondLargest = -1
```

### Read 12

```
12 > -1

secondLargest = -1
largest = 12
```

State:

```
largest = 12
secondLargest = -1
```

---

### Read 35

```
35 > 12

secondLargest = 12
largest = 35
```

State:

```
largest = 35
secondLargest = 12
```

---

### Read 1

```
1 > 35 ? No

1 < 35
1 > 12 ? No
```

No change.

---

### Read 10

```
10 < 35

10 > 12 ? No
```

No change.

---

### Read 34

```
34 < 35
34 > 12

Yes

secondLargest = 34
```

State:

```
largest = 35
secondLargest = 34
```

---

### Read 1

No change.

Final Answer:

```
34
```

---

### Example 2

```
Input:
10 5 10
```

Initially

```
largest = -1
secondLargest = -1
```

Read 10

```
largest = 10
secondLargest = -1
```

Read 5

```
5 < 10
5 > -1

secondLargest = 5
```

Read 10

```
10 > 10 ? No

10 < 10 ? No
```

Final Answer:

```
5
```

---

### Example 3

```
Input:
10 10 10
```

First element

```
largest = 10
secondLargest = -1
```

Remaining elements

Neither condition is satisfied.

Answer:

```
-1
```

---

# IMPORTANT CODE SNIPPETS:

### New Maximum Found

```cpp
if(arr[i] > largest) {

    secondLargest = largest;
    largest = arr[i];
}
```

---

### Update Second Largest

```cpp
else if(arr[i] < largest &&
        arr[i] > secondLargest) {

    secondLargest = arr[i];
}
```

---

### Ignore Duplicate Maximum

```cpp
arr[i] < largest
```

This single condition prevents duplicate maximum values from becoming the second largest.

---

# COMMON MISTAKES:

### Mistake 1

Using

```cpp
arr[i] >= largest
```

Duplicate maximum values overwrite the second largest.

---

### Mistake 2

Forgetting

```cpp
arr[i] < largest
```

Then duplicate maximum values become the second largest.

---

### Mistake 3

Immediately sorting the array.

Interviewers usually expect the linear solution.

---

### Mistake 4

Updating variables in the wrong order.

Wrong:

```cpp
largest = arr[i];
secondLargest = largest;
```

Old largest is lost.

Correct:

```cpp
secondLargest = largest;
largest = arr[i];
```

---

### Mistake 5

Initializing

```cpp
largest = 0;
```

This only works because constraints contain positive integers.

For a general solution, use:

```cpp
INT_MIN
```

---

# WHY I MIGHT FORGET THIS:

The only thing to remember is:

> **Whenever a new largest appears, the old largest automatically becomes the second largest.**

```
secondLargest = largest;
largest = current;
```

Also remember:

Duplicate largest ≠ Second largest

Hence,

```cpp
current < largest
```

is mandatory.

---

# INTERVIEW FLOW:

> We only need the largest and second largest elements, so sorting is unnecessary. I maintain two variables while traversing the array once. If the current element is greater than the largest, I move the previous largest into the second largest and update the largest. Otherwise, if it is smaller than the largest but greater than the current second largest, I update the second largest. The condition `current < largest` ensures duplicate maximum values are ignored. This gives an **O(N)** time and **O(1)** space solution.

---

# TIME COMPLEXITY:

### Optimal

```
O(N)
```

### Reason

The array is traversed exactly once.

Each element is processed in constant time.

---

# SPACE COMPLEXITY:

```
O(1)
```

Only two extra variables are used.

---

# EDGE CASES:

### All Elements Same

```
10 10 10

Output:
-1
```

---

### Two Elements

```
5 2

Output:
2
```

---

### Duplicate Largest

```
5 5 3

Output:
3
```

---

### Increasing Order

```
1 2 3 4 5

Output:
4
```

---

### Decreasing Order

```
9 8 7 6

Output:
8
```

---

### Minimum Size Array

```
2 elements
```

Works correctly.

---

# PATTERN RECOGNITION:

Whenever the question asks:

- Largest element
- Second largest
- Third largest
- Smallest
- Second smallest
- Top K (small K)
- Maximum/Minimum tracking
- Find best candidate while scanning

Think:

> **Maintain running extremes in one traversal instead of sorting.**

General Template:

```cpp
best = ...
secondBest = ...

for(each element){

    if(new best){

        secondBest = best;
        best = current;
    }

    else if(better than secondBest){

        secondBest = current;
    }
}
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int getSecondLargest(vector<int> &arr) {

        int largest = -1;
        int secondLargest = -1;

        for (int x : arr) {

            if (x > largest) {
                secondLargest = largest;
                largest = x;
            }
            else if (x < largest && x > secondLargest) {
                secondLargest = x;
            }
        }

        return secondLargest;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int largest = -1;
```

Stores the largest element seen so far.

---

```cpp
int secondLargest = -1;
```

Stores the largest element that is strictly smaller than `largest`.

---

```cpp
for(int x : arr)
```

Traverse every element exactly once.

---

```cpp
if(x > largest)
```

A new maximum is found.

---

```cpp
secondLargest = largest;
```

Preserve the old maximum before replacing it.

---

```cpp
largest = x;
```

Update the maximum.

---

```cpp
else if(x < largest && x > secondLargest)
```

Update the second largest only if:
- it is different from the largest
- it improves the current second largest

---

```cpp
return secondLargest;
```

Returns `-1` automatically if no distinct second largest exists.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Maintain Multiple Extremes
- Keep two variables:
  - `largest`
  - `secondLargest`
- If a **new largest** appears:

```cpp
secondLargest = largest;
largest = current;
```

- Otherwise, if

```cpp
current < largest &&
current > secondLargest
```

update `secondLargest`.

- The condition

```cpp
current < largest
```

is the key to ignoring duplicate maximum values.

### Complexity

- **Time:** `O(N)`
- **Space:** `O(1)`
