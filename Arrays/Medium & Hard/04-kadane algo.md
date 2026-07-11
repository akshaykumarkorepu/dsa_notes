
# Maximum Subarray Sum (Kadane's Algorithm)

---

# NOTE

## PROBLEM

Given an integer array `arr[]`, find the **maximum sum of a contiguous subarray**.

**Example**

```text
Input:
arr = [2,3,-8,7,-1,2,3]

Output:
11

Maximum Sum Subarray = [7,-1,2,3]
```

---

## PATTERN

**Kadane's Algorithm (Running Sum / DP on Arrays)**

---

## WHY THIS PATTERN

The problem asks for the **maximum sum of a contiguous subarray**.

The brute force solution generates every subarray.

Instead of checking every subarray, Kadane makes one key observation:

> **A negative running sum can never improve any future subarray.**

Once we know this, we can discard negative prefixes immediately and solve the problem in one traversal.

---

## CORE IDEA

Maintain a running sum.

```text
Add current element
        ↓
Update maximum answer
        ↓
If running sum becomes negative
        ↓
Discard it
        ↓
Start a new subarray
```

Everything in Kadane comes from remembering one sentence:

> **Negative Prefix Never Helps.**

---

# BRUTE FORCE

## Intuition

We don't know which subarray gives the maximum sum.

So generate **every possible subarray**, compute its sum, and keep the largest.

---

## Flow

Outer loop chooses the starting index.

```cpp
for(int i=0;i<n;i++)
```

Inner loop chooses the ending index.

```cpp
for(int j=i;j<n;j++)
```

Every pair `(i,j)` represents one subarray.

Instead of recomputing the sum every time, keep extending it.

```cpp
sum += arr[j];
```

Whenever a larger sum is found,

```cpp
maxSum = max(maxSum,sum);
```

---

## Dry Run

```text
arr = [2,3,-8]

i=0

j=0

sum=2

max=2

----------------

j=1

sum=5

max=5

----------------

j=2

sum=-3

max=5

----------------

i=1

sum=0

j=1

sum=3

max=5

----------------

j=2

sum=-5

----------------

i=2

sum=-8
```

Answer

```text
5
```

---

## Brute Force Code

```cpp
class Solution {
public:
    int maxSubarraySum(vector<int> &arr) {

        int n = arr.size();
        int maxSum = INT_MIN;

        for(int i=0;i<n;i++)
        {
            int sum=0;

            for(int j=i;j<n;j++)
            {
                sum += arr[j];

                maxSum=max(maxSum,sum);
            }
        }

        return maxSum;
    }
};
```

---

# OPTIMAL APPROACH

(Kadane's Algorithm)

---

## Observation

Suppose

```text
Current Sum = -4

Next Element = 8
```

Two choices

Continue

```text
-4 + 8 = 4
```

Start New

```text
8
```

Clearly

```text
8 > 4
```

So carrying a negative sum is useless.

Hence,

```cpp
if(sum<0)
    sum=0;
```

---

## ALGORITHM

Initialize

```cpp
sum=0;
maxSum=INT_MIN;
```

Traverse the array.

For every element

```cpp
sum += arr[i];
```

Update answer

```cpp
maxSum=max(maxSum,sum);
```

If running sum becomes negative,

```cpp
sum=0;
```

Return

```cpp
maxSum;
```

---

## DRY RUN

```text
Array

[2,3,-8,7,-1,2,3]
```

Initial

```text
sum=0

max=-∞
```

Element

```text
2
```

```text
sum=2

max=2
```

----------------

Element

```text
3
```

```text
sum=5

max=5
```

----------------

Element

```text
-8
```

```text
sum=-3

max=5

sum becomes negative

Reset

sum=0
```

----------------

Element

```text
7
```

```text
sum=7

max=7
```

----------------

Element

```text
-1
```

```text
sum=6

max=7
```

----------------

Element

```text
2
```

```text
sum=8

max=8
```

----------------

Element

```text
3
```

```text
sum=11

max=11
```

Answer

```text
11
```

---

## IMPORTANT OBSERVATIONS

- Negative running sums are never useful.
- Always update `maxSum` before resetting `sum`.
- Initialize `maxSum = INT_MIN` to handle all-negative arrays.
- Kadane works only for **contiguous** subarrays.

---

## IMPORTANT CODE SNIPPETS

Running Sum

```cpp
sum += arr[i];
```

Update Answer

```cpp
maxSum=max(maxSum,sum);
```

Discard Negative Prefix

```cpp
if(sum<0)
    sum=0;
```

---

## COMMON MISTAKES

Initializing

```cpp
maxSum=0;
```

Fails for

```text
[-2,-5]
```

---

Resetting

```cpp
sum=0;
```

before updating `maxSum`.

Wrong order.

---

Thinking Kadane works for subsequences.

It only works for contiguous subarrays.

---

## WHY I MIGHT FORGET THIS

Don't memorize the algorithm.

Remember the observation.

> **Negative Prefix Never Helps**

Everything else follows naturally.

---

## INTERVIEW FLOW

**Step 1**

Explain brute force.

> Generate every subarray.
> Calculate its sum.
> Keep the maximum.

---

**Step 2**

Observation.

> A negative running sum can never improve any future subarray.

---

**Step 3**

Kadane.

> Maintain a running sum.
> Update maximum.
> If running sum becomes negative, discard it.

---

## TIME COMPLEXITY

### Brute Force

Outer Loop

```text
n
```

Inner Loop

```text
n
```

Overall

```text
O(n²)
```

---

### Optimal

Single traversal.

```text
O(n)
```

---

## SPACE COMPLEXITY

Brute Force

```text
O(1)
```

Optimal

```text
O(1)
```

---

## EDGE CASES

All negative numbers

```text
[-2,-5]
```

Single element

```text
[7]
```

All positive

```text
[1,2,3]
```

Maximum subarray at beginning.

Maximum subarray at end.

Entire array is the answer.

---

## PATTERN RECOGNITION

Use Kadane when you see

- Maximum/Minimum Sum
- Contiguous Subarray
- Running contribution matters
- One traversal is possible

---

# Clean C++ Code

```cpp
class Solution {
public:
    int maxSubarraySum(vector<int> &arr) {

        int sum=0;
        int maxSum=INT_MIN;

        for(int i=0;i<arr.size();i++)
        {
            sum += arr[i];

            maxSum=max(maxSum,sum);

            if(sum<0)
                sum=0;
        }

        return maxSum;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
sum += arr[i];
```

Extend the current subarray.

---

```cpp
maxSum=max(maxSum,sum);
```

Check whether the current subarray is the best one seen so far.

---

```cpp
if(sum<0)
    sum=0;
```

Discard the negative prefix because it can never improve a future answer.

---

# Easy-to-Remember Summary

```text
Generate Every Subarray

↓

Brute Force

↓

Observation

Negative Prefix Never Helps

↓

Keep Running Sum

↓

Update Maximum

↓

Running Sum < 0 ?

↓

Reset to 0

↓

Kadane

O(n)
```

---

# ================================================================

# PRINTING THE MAXIMUM SUBARRAY

---

# NOTE

## PROBLEM

Return the **maximum sum contiguous subarray itself**, not just its sum.

Example

```text
Input

[2,3,-8,7,-1,2,3]

Output

[7,-1,2,3]
```

---

## PATTERN

**Kadane's Algorithm + Index Tracking**

---

## WHY THIS PATTERN

Kadane already gives the maximum sum in O(n).

To print the subarray, we only need to remember where it starts and ends.

---

## CORE IDEA

Track two things.

1. Current Candidate Subarray
2. Best Subarray

Whenever the current candidate becomes the best answer,

save its boundaries.

---

# BRUTE FORCE

## Intuition

Generate every subarray.

Whenever a larger sum is found,

store

```cpp
start=i;

end=j;
```

Finally print from

```text
start

↓

end
```

---

## Brute Force Code

```cpp
vector<int> maxSubarray(vector<int>& arr) {

    int n=arr.size();

    int maxSum=INT_MIN;

    int start=0;
    int end=0;

    for(int i=0;i<n;i++)
    {
        int sum=0;

        for(int j=i;j<n;j++)
        {
            sum += arr[j];

            if(sum>maxSum)
            {
                maxSum=sum;

                start=i;

                end=j;
            }
        }
    }

    vector<int> ans;

    for(int i=start;i<=end;i++)
        ans.push_back(arr[i]);

    return ans;
}
```

---

# OPTIMAL APPROACH

## Core Idea

Kadane never explicitly generates every subarray.

So we don't know where the current candidate started.

Hence we introduce

```cpp
tempStart
```

---

## Variables

```text
tempStart

↓

Current Candidate Starts Here

-----------------------

start

↓

Best Subarray Starts Here

-----------------------

end

↓

Best Subarray Ends Here
```

---

## ALGORITHM

For every element

```cpp
sum += arr[i];
```

If current candidate becomes the best answer

```cpp
start=tempStart;

end=i;
```

If running sum becomes negative

```cpp
sum=0;

tempStart=i+1;
```

Finally print

```text
start

↓

end
```

---

## DRY RUN

```text
Array

[2,3,-8,7,-1,2,3]
```

| i | arr[i] | sum | tempStart | start | end | max |
|---|--------|-----|-----------|-------|-----|-----|
|0|2|2|0|0|0|2|
|1|3|5|0|0|1|5|
|2|-8|-3|0|0|1|5|
|Reset|-|-|3|-|-|-|
|3|7|7|3|3|3|7|
|4|-1|6|3|3|3|7|
|5|2|8|3|3|5|8|
|6|3|11|3|3|6|11|

Final

```text
start=3

end=6
```

Subarray

```text
[7,-1,2,3]
```

---

## IMPORTANT OBSERVATIONS

`tempStart`

tracks the beginning of the current candidate.

`start`

tracks the beginning of the best answer.

`end`

tracks the end of the best answer.

Never update `start` when resetting.

Only update `tempStart`.

---

## IMPORTANT CODE SNIPPETS

Current Candidate becomes Best

```cpp
if(sum>maxSum)
{
    maxSum=sum;

    start=tempStart;

    end=i;
}
```

Discard Current Candidate

```cpp
if(sum<0)
{
    sum=0;

    tempStart=i+1;
}
```

Print

```cpp
vector<int> ans;

for(int i=start;i<=end;i++)
    ans.push_back(arr[i]);

return ans;
```

---

## COMMON MISTAKES

Doing

```cpp
start=i;
```

instead of

```cpp
start=tempStart;
```

---

Updating

```cpp
start
```

when sum becomes negative.

Wrong.

Only update

```cpp
tempStart.
```

---

Resetting before updating answer.

Wrong order.

---

## WHY I MIGHT FORGET THIS

Remember the three indices.

```text
tempStart

↓

Current Candidate

----------------

start

↓

Best Start

----------------

end

↓

Best End
```

---

## INTERVIEW FLOW

**Step 1**

Explain brute force.

Store

```cpp
start=i;

end=j;
```

---

**Step 2**

Observation.

Negative Prefix Never Helps.

---

**Step 3**

Explain Kadane.

Introduce

```cpp
tempStart
```

---

**Step 4**

Explain the two events.

Running Sum Negative

↓

Discard Candidate

↓

Update

```cpp
tempStart=i+1;
```

Current Candidate becomes Best

↓

Store

```cpp
start=tempStart;

end=i;
```

---

## TIME COMPLEXITY

Brute Force

```text
O(n²)
```

Optimal

```text
O(n)
```

---

## SPACE COMPLEXITY

Both

```text
O(1)
```

(ignoring returned vector)

---

## EDGE CASES

All negative numbers.

Single element.

Entire array is the answer.

Maximum subarray at beginning.

Maximum subarray at end.

---

## PATTERN RECOGNITION

Whenever the question says

- Print Maximum Subarray
- Return Indices
- Return the Subarray

Think

```text
Kadane

+

tempStart

+

start

+

end
```

---

# Clean C++ Code

```cpp
vector<int> maxSubarray(vector<int>& arr) {

    int sum=0;
    int maxSum=INT_MIN;

    int start=0;
    int end=0;
    int tempStart=0;

    for(int i=0;i<arr.size();i++)
    {
        sum += arr[i];

        if(sum>maxSum)
        {
            maxSum=sum;

            start=tempStart;

            end=i;
        }

        if(sum<0)
        {
            sum=0;

            tempStart=i+1;
        }
    }

    vector<int> ans;

    for(int i=start;i<=end;i++)
        ans.push_back(arr[i]);

    return ans;
}
```

---

# Intuition Behind Every Important Line

```cpp
sum += arr[i];
```

Extend the current candidate.

---

```cpp
if(sum>maxSum)
```

The current candidate has become the best subarray.

---

```cpp
start=tempStart;

end=i;
```

Save the boundaries of the best subarray.

---

```cpp
if(sum<0)
```

Current candidate cannot help any future subarray.

---

```cpp
tempStart=i+1;
```

The next element becomes the beginning of the next candidate.

---

# Easy-to-Remember Summary

```text
BRUTE

Generate Every Subarray

↓

Store

start=i

end=j

↓

Print

-----------------------------------

KADANE

Negative Prefix Never Helps

↓

Running Sum

↓

Current Candidate Starts at

tempStart

↓

Candidate becomes Best

↓

start=tempStart

end=i

↓

Print start → end
```
