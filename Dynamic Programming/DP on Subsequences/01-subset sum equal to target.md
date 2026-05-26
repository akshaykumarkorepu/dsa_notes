
# Note

# PROBLEM:
Subset Sum Problem

# PATTERN:
Take / Not Take DP Pattern

# WHY THIS PATTERN:
At every element we have only 2 choices:
- Take the element
- Do not take the element

This exact decision-making creates DP.

---

# QUESTION UNDERSTANDING

Given:
- an array `arr[]`
- a target `sum`

We need to determine:

```text
Is there any subset whose sum becomes exactly equal to target?
```

Return:
- `true` → possible
- `false` → impossible

---

# EXAMPLE

```cpp
arr = [3,4,5]
sum = 9
```

Possible subset:

```cpp
4 + 5 = 9
```

So answer:

```cpp
true
```

---

# SHORTCUT DP THINKING

Whenever you see:

```text
Take element / skip element
```

Think:

# Take / Not Take DP

---

# MAIN RECURSION IDEA

We define:

```cpp
solve(index,target)
```

Meaning:

```text
Can we form "target"
using elements from 0 to index?
```

---

# RECURSION FLOW

At every index:

## Option 1 → Not Take

```cpp
solve(index-1,target)
```

Target remains same.

---

## Option 2 → Take

Only possible if:

```cpp
arr[index] <= target
```

Then:

```cpp
solve(index-1,target-arr[index])
```

---

# BASE CASES

---

# BASE CASE 1

```cpp
if(target == 0)
    return true;
```

Meaning:

```text
We successfully formed target.
```

---

# BASE CASE 2

```cpp
if(index == 0)
    return arr[0] == target;
```

Meaning:

```text
Only one element left.
Check whether it alone forms target.
```

---

# WHY I MIGHT FORGET

```text
I forget what state means.

Always remember:

solve(index,target)
=
Can I make target using elements till index?
```

---

# RECURSION CODE

```cpp
class Solution {
public:

    bool solve(int index,
               int target,
               vector<int>& arr) {

        // Base Case
        if(target == 0)
            return true;

        // Base Case
        if(index == 0)
            return arr[0] == target;

        // Not Take
        bool notTake = solve(index - 1,
                             target,
                             arr);

        // Take
        bool take = false;

        if(arr[index] <= target)
            take = solve(index - 1,
                         target - arr[index],
                         arr);

        return take || notTake;
    }

    bool isSubsetSum(vector<int>& arr,
                     int sum) {

        int n = arr.size();

        return solve(n - 1, sum, arr);
    }
};
```

---

# RECURSION DRY RUN

```cpp
arr = [1,2,3]
sum = 4
```

Start:

```cpp
solve(2,4)
```

Element = 3

---

## TAKE 3

```cpp
solve(1,1)
```

Element = 2

Cannot take because:

```cpp
2 > 1
```

So:

```cpp
solve(0,1)
```

Now:

```cpp
arr[0] == target
1 == 1
```

TRUE.

So subset exists:

```cpp
1 + 3 = 4
```

---

# RECURSION TC

```text
O(2^n)
```

---

# RECURSION SC

```text
O(n)
```

---

# WHY RECURSION IS SLOW

Same states get recomputed.

Example:

```cpp
solve(3,5)
```

can be called multiple times.

This is:

# Overlapping Subproblems

---

# MEMOIZATION IDEA

Store already solved states.

Before solving:

```cpp
if(dp[index][target] != -1)
    return dp[index][target];
```

---

# DP STATE

```cpp
dp[index][target]
```

Meaning:

```text
Can we form target
using elements till index?
```

---

# MEMOIZATION CODE

```cpp
class Solution {
public:

    bool solve(int index,
               int target,
               vector<int>& arr,
               vector<vector<int>>& dp) {

        if(target == 0)
            return true;

        if(index == 0)
            return arr[0] == target;

        if(dp[index][target] != -1)
            return dp[index][target];

        bool notTake = solve(index - 1,
                             target,
                             arr,
                             dp);

        bool take = false;

        if(arr[index] <= target)
            take = solve(index - 1,
                         target - arr[index],
                         arr,
                         dp);

        return dp[index][target]
               = take || notTake;
    }

    bool isSubsetSum(vector<int>& arr,
                     int sum) {

        int n = arr.size();

        vector<vector<int>> dp(
            n,
            vector<int>(sum + 1, -1)
        );

        return solve(n - 1,
                     sum,
                     arr,
                     dp);
    }
};
```

---

# MEMOIZATION DRY RUN

Suppose:

```cpp
solve(2,4)
```

gets solved once.

Store:

```cpp
dp[2][4] = true
```

Next time if:

```cpp
solve(2,4)
```

comes again:

Directly return from DP.

No recursion again.

---

# MEMOIZATION TC

```text
O(n * sum)
```

---

# MEMOIZATION SC

DP Table:

```text
O(n * sum)
```

Recursion Stack:

```text
O(n)
```

---

# TABULATION IDEA

Convert recursion into iterative DP.

---

# TABULATION STATE

```cpp
dp[index][target]
```

Meaning remains SAME.

---

# TABULATION BASE CASES (VERY IMPORTANT)

---

# BASE CASE 1

```cpp
dp[i][0] = true
```

for all rows.

---

# WHY?

Question:

```text
Can we form target 0?
```

YES.

How?

Take NO elements.

Empty subset.

---

# EXAMPLE

```cpp
arr = [1,2,3]
```

Can we make 0?

YES.

Take nothing.

So:

```text
        0 1 2 3 4

0       T
1       T
2       T
```

Entire first column = TRUE.

---

# BASE CASE 2

```cpp
dp[0][arr[0]] = true;
```

---

# WHY?

First row means:

```text
Only first element is available.
```

---

# EXAMPLE

```cpp
arr = [1,2,3]
```

At row 0:

Only available element:

```cpp
1
```

What sums possible?

```text
0 → take nothing
1 → take 1
```

Impossible:

```text
2
3
4
```

So row becomes:

```text
        0 1 2 3 4

0       T T F F F
```

---

# TABULATION TRANSITION

---

# NOT TAKE

```cpp
dp[index-1][target]
```

---

# TAKE

```cpp
dp[index-1][target-arr[index]]
```

---

# FINAL

```cpp
dp[index][target]
=
take || notTake
```

---

# TABULATION CODE

```cpp
class Solution {
public:

    bool isSubsetSum(vector<int>& arr,
                     int sum) {

        int n = arr.size();

        vector<vector<bool>> dp(
            n,
            vector<bool>(sum + 1, false)
        );

        // Base Case 1
        for(int i = 0; i < n; i++) {
            dp[i][0] = true;
        }

        // Base Case 2
        if(arr[0] <= sum)
            dp[0][arr[0]] = true;

        // Fill table
        for(int index = 1;
            index < n;
            index++) {

            for(int target = 1;
                target <= sum;
                target++) {

                bool notTake =
                    dp[index-1][target];

                bool take = false;

                if(arr[index] <= target)
                    take =
                    dp[index-1]
                      [target-arr[index]];

                dp[index][target]
                    = take || notTake;
            }
        }

        return dp[n-1][sum];
    }
};
```

---

# TABULATION DRY RUN

```cpp
arr = [1,2,3]
sum = 4
```

Initial:

```text
        0 1 2 3 4

0       T T F F F
1       T F F F F
2       T F F F F
```

After processing 2:

```text
        0 1 2 3 4

0       T T F F F
1       T T T T F
```

After processing 3:

```text
        0 1 2 3 4

0       T T F F F
1       T T T T F
2       T T T T T
```

Answer:

```cpp
dp[2][4] = true
```

---

# TABULATION TC

```text
O(n * sum)
```

---

# TABULATION SC

```text
O(n * sum)
```

---

# SPACE OPTIMIZATION IDEA

Observe:

```text
Current row only depends
on previous row.
```

So entire matrix unnecessary.

Store only:
- prev row
- curr row

---

# SPACE OPTIMIZED CODE

```cpp
class Solution {
public:

    bool isSubsetSum(vector<int>& arr,
                     int sum) {

        int n = arr.size();

        vector<bool> prev(sum + 1, false);

        prev[0] = true;

        if(arr[0] <= sum)
            prev[arr[0]] = true;

        for(int index = 1;
            index < n;
            index++) {

            vector<bool> curr(sum + 1, false);

            curr[0] = true;

            for(int target = 1;
                target <= sum;
                target++) {

                bool notTake =
                    prev[target];

                bool take = false;

                if(arr[index] <= target)
                    take =
                    prev[target-arr[index]];

                curr[target]
                    = take || notTake;
            }

            prev = curr;
        }

        return prev[sum];
    }
};
```

---

# SPACE OPTIMIZATION DRY RUN

Initial:

```text
prev:
T T F F F
```

After processing 2:

```text
curr:
T T T T F
```

After processing 3:

```text
curr:
T T T T T
```

Answer:

```cpp
prev[4] = true
```

---

# SPACE OPTIMIZATION TC

```text
O(n * sum)
```

---

# SPACE OPTIMIZATION SC

```text
O(sum)
```

---

# INTERVIEW EXPLANATION FLOW

If this question comes in interview:

---

# STEP 1

Say:

```text
At every element,
I have two choices:
take or not take.
```

So recursion identified.

---

# STEP 2

Define state:

```cpp
solve(index,target)
```

Meaning:

```text
Can I form target
using elements till index?
```

---

# STEP 3

Explain transitions:
- take
- notTake

---

# STEP 4

Explain overlapping subproblems.

Move to memoization.

---

# STEP 5

Convert memoization to tabulation.

Same state:
- rows = index
- columns = target

---

# STEP 6

Explain MOST IMPORTANT observation:

```text
Current row depends only on previous row.
```

Hence space optimization.

---

# MOST IMPORTANT MEMORY TRICK

Whenever you see:

```text
Take / Not Take
```

Think:

# Subsequence DP

Usually:
- subset sum
- knapsack
- partition problems
- target sum

All use SAME pattern.
````
