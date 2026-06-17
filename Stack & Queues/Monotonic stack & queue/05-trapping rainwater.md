
## PROBLEM:

Given an array where each element represents the height of a building, find the total amount of rainwater that can be trapped after raining.

Example:

```text
arr = [3,0,1,0,4]

          █
█         █
█    █    █
█    █    █
█ █  █ █  █
------------
3 0 1 0 4
```

Water trapped:

```text
Index 1 → 3
Index 2 → 2
Index 3 → 3

Total = 8
```

---

## PATTERN:

```text
1. Prefix/Suffix Maximums
2. Two Pointers
3. Boundary Based Water Trapping
```

This is NOT a Stack question.

The optimal solution uses:

```text
Two Pointers + Running Maximums
```

---

## WHY THIS PATTERN:

For water to stay at an index:

```text
Need a boundary on left
Need a boundary on right
```

Water level is determined by:

```text
min(left boundary, right boundary)
```

Therefore:

```text
water(i)
=
min(leftMax,rightMax)
-
height[i]
```

Entire problem revolves around efficiently finding:

```text
leftMax
rightMax
```

---

## CORE IDEA:

For every index:

```text
water(i)
=
min(leftMax,rightMax)
-
arr[i]
```

Where:

```text
leftMax  = tallest wall on left
rightMax = tallest wall on right
```

Example:

```text
3 0 1 0 4
  ^
```

```text
leftMax = 3
rightMax = 4

water = min(3,4)-0
       = 3
```

---

## BRUTE FORCE:

### Intuition

For every index:

```text
Find tallest wall on left

Find tallest wall on right

Apply formula
```

### Algorithm

For every index:

```text
leftMax  = maximum from 0 to i

rightMax = maximum from i to n-1

water += min(leftMax,rightMax)-arr[i]
```

### Code

```cpp
int maxWater(vector<int>& arr) {

    int n = arr.size();
    int water = 0;

    for(int i=0;i<n;i++) {

        int leftMax = arr[i];

        for(int j=0;j<=i;j++) {
            leftMax = max(leftMax, arr[j]);
        }

        int rightMax = arr[i];

        for(int j=i;j<n;j++) {
            rightMax = max(rightMax, arr[j]);
        }

        water += min(leftMax,rightMax) - arr[i];
    }

    return water;
}
```

### Dry Run

```text
arr = [3,0,1,0,4]
```

For index 2:

```text
3 0 1 0 4
    ^
```

```text
leftMax = max(3,0,1)
        = 3

rightMax = max(1,0,4)
         = 4

water

=
min(3,4)-1

=
2
```

### Time Complexity

```text
O(n²)
```

Reason:

For every index:

```text
Find leftMax  -> O(n)

Find rightMax -> O(n)
```

Repeated for n indices.

### Space Complexity

```text
O(1)
```

### Problem With Brute Force

We repeatedly compute:

```text
leftMax

leftMax

leftMax
```

again and again.

Example:

```text
3 0 1 0 4
```

For:

```text
i=1

leftMax=3
```

For:

```text
i=2

leftMax=3
```

For:

```text
i=3

leftMax=3
```

Repeated work.

---

## BETTER APPROACH (PREFIX/SUFFIX ARRAYS)

### Idea

Store all leftMax values once.

Store all rightMax values once.

### Build

```text
arr

3 0 1 0 4
```

```text
leftMax

3 3 3 3 4
```

```text
rightMax

4 4 4 4 4
```

Now:

```text
water

=
min(leftMax[i],rightMax[i])
-
arr[i]
```

in O(1).

### Code

```cpp
int maxWater(vector<int>& arr) {

    int n = arr.size();

    vector<int> leftMax(n);
    vector<int> rightMax(n);

    leftMax[0] = arr[0];

    for(int i=1;i<n;i++) {
        leftMax[i] = max(leftMax[i-1], arr[i]);
    }

    rightMax[n-1] = arr[n-1];

    for(int i=n-2;i>=0;i--) {
        rightMax[i] = max(rightMax[i+1], arr[i]);
    }

    int water = 0;

    for(int i=0;i<n;i++) {
        water += min(leftMax[i], rightMax[i]) - arr[i];
    }

    return water;
}
```

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

For:

```cpp
leftMax[]
rightMax[]
```

---

## OPTIMAL APPROACH:

### Key Observation

Suppose:

```text
leftMax = 4
rightMax = 10
```

Then:

```text
water level

=
min(4,10)

=
4
```

Even if:

```text
rightMax = 100
```

Water level is still:

```text
4
```

Therefore:

```text
When leftMax < rightMax

water is determined by leftMax only.
```

Similarly:

```text
When rightMax <= leftMax

water is determined by rightMax only.
```

This removes the need for:

```text
leftMax[]
rightMax[]
```

arrays.

---

## ALGORITHM:

Maintain:

```cpp
left
right

leftMax
rightMax
```

Initialize:

```cpp
left = 0
right = n-1

leftMax = 0
rightMax = 0
```

While:

```cpp
left <= right
```

### Case 1

```cpp
arr[left] <= arr[right]
```

Process left side.

If:

```cpp
arr[left] >= leftMax
```

Update:

```cpp
leftMax = arr[left]
```

Else:

```cpp
water += leftMax-arr[left]
```

Move:

```cpp
left++
```

### Case 2

```cpp
arr[left] > arr[right]
```

Process right side.

If:

```cpp
arr[right] >= rightMax
```

Update:

```cpp
rightMax = arr[right]
```

Else:

```cpp
water += rightMax-arr[right]
```

Move:

```cpp
right--
```

---

## DRY RUN:

```text
arr = [5,4,1,2]
```

Initial:

```text
5 4 1 2

L     R

leftMax=0
rightMax=0
water=0
```

### Iteration 1

```cpp
if(5 <= 2)
```

FALSE

Process right.

```cpp
rightMax = 2
right--
```

State:

```text
rightMax=2
water=0
```

### Iteration 2

```cpp
if(5 <= 1)
```

FALSE

Process right.

```cpp
water += 2-1
```

```text
water=1
```

```cpp
right--
```

### Iteration 3

```cpp
if(5 <= 4)
```

FALSE

Process right.

```cpp
rightMax=4
right--
```

### Iteration 4

```cpp
if(5 <= 5)
```

TRUE

Process left.

```cpp
leftMax=5
left++
```

Stop.

Answer:

```text
1
```

---

## IMPORTANT CODE SNIPPETS:

### Water Formula

```cpp
water += min(leftMax,rightMax)-arr[i];
```

### Prefix Maximum

```cpp
leftMax[i] = max(leftMax[i-1], arr[i]);
```

### Suffix Maximum

```cpp
rightMax[i] = max(rightMax[i+1], arr[i]);
```

### Two Pointer Decision

```cpp
if(arr[left] <= arr[right])
```

This is the heart of the optimal solution.

### Water Calculation

```cpp
water += leftMax-arr[left];
```

or

```cpp
water += rightMax-arr[right];
```

---

## COMMON MISTAKES:

### Mistake 1

Memorizing:

```cpp
if(arr[left] <= arr[right])
```

without understanding it.

Remember:

```text
Smaller boundary determines water.
```

### Mistake 2

Thinking:

```text
Need exact rightMax
before processing left.
```

Wrong.

The smaller side already fixes the answer.

### Mistake 3

Using:

```cpp
max(leftMax,rightMax)
```

instead of:

```cpp
min(leftMax,rightMax)
```

### Mistake 4

Forgetting:

```cpp
water += leftMax-arr[left]
```

only when:

```cpp
arr[left] < leftMax
```

---

## WHY I MIGHT FORGET THIS:

Most people focus on:

```cpp
if(arr[left] <= arr[right])
```

and memorize it.

Instead remember:

```text
Water level is decided by the SMALLER boundary.
```

Everything else follows automatically.

---

## INTERVIEW FLOW:

### Step 1

State formula:

```text
water(i)
=
min(leftMax,rightMax)
-
height[i]
```

### Step 2

Brute Force

For every index:

```text
Find leftMax

Find rightMax
```

```text
O(n²)
```

### Step 3

Optimization

Repeated computation of:

```text
leftMax
rightMax
```

Use:

```text
Prefix Max
Suffix Max
```

```text
O(n)
time

O(n)
space
```

### Step 4

Further Optimization

Ask:

```text
Do I really need arrays?
```

Observation:

```text
Smaller boundary determines water.
```

Use:

```text
Two Pointers
```

### Step 5

Present Optimal

```text
O(n)
time

O(1)
space
```

---

## TIME COMPLEXITY:

### Brute Force

```text
O(n²)
```

Reason:

```text
For every index

scan left
scan right
```

### Better (Prefix/Suffix)

```text
O(n)
```

Reason:

```text
Build leftMax

Build rightMax

Compute answer
```

Three linear traversals.

### Optimal (Two Pointers)

```text
O(n)
```

Reason:

```text
Each pointer moves only once.

Every index processed once.
```

---

## SPACE COMPLEXITY:

### Brute Force

```text
O(1)
```

### Better

```text
O(n)
```

For:

```cpp
leftMax[]
rightMax[]
```

### Optimal

```text
O(1)
```

Only:

```cpp
left
right
leftMax
rightMax
water
```

---

## EDGE CASES:

### Increasing Heights

```text
1 2 3 4
```

Answer:

```text
0
```

### Decreasing Heights

```text
4 3 2 1
```

Answer:

```text
0
```

### All Same Heights

```text
3 3 3 3
```

Answer:

```text
0
```

### Single Valley

```text
3 0 3
```

Answer:

```text
3
```

### Deep Valley

```text
5 0 0 0 5
```

Answer:

```text
15
```

---

## PATTERN RECOGNITION:

You should think of this pattern when:

```text
For every index:

Need information from left side

AND

Need information from right side
```

Especially when the formula looks like:

```text
Something involving

min(left info, right info)

or

max(left info, right info)
```

Typical clues:

```text
Rain Water Trapping

Buildings

Elevation Map

Boundary Based Water Storage

Left Max + Right Max
```

---

# Clean C++ Code (Optimal)

```cpp
class Solution {
public:
    int maxWater(vector<int>& arr) {

        int left = 0;
        int right = arr.size() - 1;

        int leftMax = 0;
        int rightMax = 0;

        int water = 0;

        while(left <= right) {

            if(arr[left] <= arr[right]) {

                if(arr[left] >= leftMax)
                    leftMax = arr[left];
                else
                    water += leftMax - arr[left];

                left++;
            }
            else {

                if(arr[right] >= rightMax)
                    rightMax = arr[right];
                else
                    water += rightMax - arr[right];

                right--;
            }
        }

        return water;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
if(arr[left] <= arr[right])
```

```text
Left boundary is smaller.

Water on left side is fixed.
```

```cpp
leftMax = arr[left];
```

```text
Found a taller wall from left.
```

```cpp
water += leftMax-arr[left];
```

```text
Current wall is lower than tallest wall.

Difference becomes trapped water.
```

```cpp
rightMax = arr[right];
```

```text
Found a taller wall from right.
```

```cpp
water += rightMax-arr[right];
```

```text
Water trapped on right side.
```

---

# Easy-to-Remember Summary

```text
Formula:

water(i)
=
min(leftMax,rightMax)
-height

Brute Force:
Find leftMax/rightMax every time
O(n²)

Better:
Store leftMax[] and rightMax[]
O(n), O(n)

Optimal:
Smaller boundary determines water.

Maintain:
left
right
leftMax
rightMax

Process the smaller side.

O(n), O(1)
```

One sentence to remember forever:

```text
The smaller boundary determines the water level.
```
````
