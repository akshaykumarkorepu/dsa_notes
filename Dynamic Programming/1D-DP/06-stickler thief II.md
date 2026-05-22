# 🚀 STICKLER THIEF II

> Classic Dynamic Programming problem based on the **Circular Pick / Not Pick Pattern**

---

# 📌 Problem Statement

A thief wants to loot houses arranged in a circle.

Constraint:

```text
Cannot loot two adjacent houses
```

BUT:

```text
First and Last houses are also adjacent
```

Find the maximum money he can loot.

---

# 📌 Pattern

| Category | Type |
|---|---|
| Pattern | Dynamic Programming |
| Sub Pattern | Pick / Not Pick |
| DP Type | 1D DP |
| Special Condition | Circular Array |

---

# 🧠 WHY THIS PATTERN?

At every house we have only 2 choices:

```text
1. Pick current house
2. Skip current house
```

This becomes:

```text
Take / Not Take DP
```

BUT:

```text
First and last houses cannot be taken together
```

So we convert:

```text
1 circular problem
```

into:

```text
2 linear House Robber problems
```

---

# 🎯 CORE IDEA

We can NEVER rob both:

```text
First House + Last House
```

So divide into TWO cases.

---

# CASE 1 → Ignore First House

Take:

```text
1 → n-1
```

---

# CASE 2 → Ignore Last House

Take:

```text
0 → n-2
```

---

# ✅ FINAL ANSWER

```text
max(
    solve(0 → n-2),
    solve(1 → n-1)
)
```

---

# 🧠 DP SHORTCUT THINKING

# STEP 1 → DEFINE STATE

```text
f(i)
=
maximum money we can rob till index i
```

---

# STEP 2 → FIND CHOICES

```text
1. Pick current house
2. Skip current house
```

---

# STEP 3 → TRANSITION

If pick:
- add current value
- move to `i-2`

If not pick:
- move to `i-1`

## Recurrence Relation

```text
f(i) = max(
            arr[i] + f(i-2),
            f(i-1)
         )
```

---

# STEP 4 → HANDLE CIRCULAR CONDITION

Instead of solving whole array:

```text
Solve:
1. Ignore First
2. Ignore Last
```

Take maximum answer.

---

# 🧠 HOW TO IDENTIFY THIS QUESTION IN INTERVIEW

Whenever you see:

```text
1. Circular Array
2. Cannot take adjacent elements
3. Maximize sum/profit
4. Pick or skip choice
```

Immediately think:

```text
House Robber II
=
Two House Robber I problems
```

---

# 🚨 EDGE CASES

```text
1. Only one house
2. Two houses
3. All houses same value
4. Large values
```

---

# 🚨 WHY PEOPLE GET STUCK

```text
1. Forgetting first and last are adjacent

2. Applying normal House Robber directly

3. Forgetting to split into two cases

4. Forgetting why pick moves to i-2

5. Confusing:
linear problem
vs
circular problem
```

---

# ==================================================
# 1️⃣ RECURSION
# ==================================================

# 🧠 RECURSION IDEA

At every index:

```text
Option 1:
Pick current house
→ arr[i] + solve(i-2)

Option 2:
Skip current house
→ solve(i-1)

Take maximum of both
```

---

# 🧠 WHY DO WE NEED start?

`start` defines:

```text
Left boundary of valid subarray
```

Example:

```text
solve(3,1)
```

means:

```text
Valid range:
1 → 3
```

Recursion should NEVER go before index `1`.

---

# 🌳 RECURSION TREE

```text
solve(2,0)

├── PICK
│   └── 3 + solve(0,0)
│
└── NOT PICK
    └── solve(1,0)
```

---

# 🌳 COMPLETE DRY RUN

```text
arr = [1,2,3,1]
```

Indices:

```text
0 1 2 3
1 2 3 1
```

---

# CASE 1 → Ignore Last House

Call:

```text
solve(2,0)
```

Valid houses:

```text
[1,2,3]
```

---

# OPTION 1 → PICK

```text
3 + solve(0,0)

= 3 + 1

= 4
```

---

# OPTION 2 → NOT PICK

```text
solve(1,0)

max(
    2 + solve(-1,0),
    solve(0,0)
)

max(2,1)

= 2
```

---

# FINAL

```text
max(4,2)
=
4
```

---

# CASE 2 → Ignore First House

Call:

```text
solve(3,1)
```

Valid houses:

```text
[2,3,1]
```

---

# FINAL

```text
3
```

---

# ✅ FINAL ANSWER

```text
max(4,3)
=
4
```

---

# ✅ RECURSION CODE

```cpp
class Solution {
public:

    int solve(int ind, int start, vector<int>& arr) {

        // Base Case 1
        if(ind < start)
            return 0;

        // Base Case 2
        if(ind == start)
            return arr[ind];

        // Pick Current House
        int pick = arr[ind] + solve(ind - 2,
                                     start,
                                     arr);

        // Skip Current House
        int not_pick = solve(ind - 1,
                             start,
                             arr);

        return max(pick, not_pick);
    }

    int rob(vector<int>& arr) {

        int n = arr.size();

        // Edge Case
        if(n == 1)
            return arr[0];

        // CASE 1 → Ignore Last
        int case1 = solve(n - 2, 0, arr);

        // CASE 2 → Ignore First
        int case2 = solve(n - 1, 1, arr);

        return max(case1, case2);
    }
};
```

---

# ⏱️ TC

```text
O(2^N)
```

---

# 🗂️ SC

```text
O(N)
```

```text
Recursion Stack
```

---

# 🚨 WHY TC IS EXPONENTIAL?

```text
Every index generates:
1. Pick call
2. Not Pick call
```

Repeated subproblems occur.

---

# ==================================================
# 2️⃣ MEMOIZATION
# ==================================================

# 🧠 IDEA

In recursion:

```text
Same subproblems repeat
```

So:
- store answers
- reuse them

---

# 📌 MEMOIZATION STEPS

```text
1. Create dp array

2. Before solving:
   check if answer already exists

3. Store answer before returning
```

---

# 🌳 MEMOIZATION DRY RUN IDEA

Suppose:

```text
solve(2,0)
```

gets calculated once.

Store:

```text
dp[2] = answer
```

Next time:

```text
Directly return dp[2]
```

No repeated recursion.

---

# ✅ MEMOIZATION CODE

```cpp
class Solution {
public:

    int solve(int ind,
              int start,
              vector<int>& arr,
              vector<int>& dp) {

        // Base Case
        if(ind < start)
            return 0;

        if(ind == start)
            return arr[ind];

        // DP Check
        if(dp[ind] != -1)
            return dp[ind];

        // Pick
        int pick = arr[ind] +
                   solve(ind - 2,
                         start,
                         arr,
                         dp);

        // Not Pick
        int not_pick = solve(ind - 1,
                             start,
                             arr,
                             dp);

        // Store & Return
        return dp[ind] = max(pick, not_pick);
    }

    int rob(vector<int>& arr) {

        int n = arr.size();

        if(n == 1)
            return arr[0];

        vector<int> dp1(n, -1);
        vector<int> dp2(n, -1);

        // Ignore Last
        int case1 = solve(n - 2,
                          0,
                          arr,
                          dp1);

        // Ignore First
        int case2 = solve(n - 1,
                          1,
                          arr,
                          dp2);

        return max(case1, case2);
    }
};
```

---

# 🔥 IMPORTANT CODE SNIPPETS

## DP CHECK

```cpp
if(dp[ind] != -1)
    return dp[ind];
```

---

## STORE ANSWER

```cpp
return dp[ind] = max(pick, not_pick);
```

---

# ⏱️ TC

```text
O(N)
```

---

# 🗂️ SC

```text
O(N) + O(N)
```

```text
DP Array + Recursion Stack
```

---

# ==================================================
# 3️⃣ TABULATION
# ==================================================

# 🧠 IDEA

Memoization:
- Top Down
- Recursive

Tabulation:
- Bottom Up
- Iterative

Build answers from smaller subproblems.

---

# 📌 TRANSITION

```text
dp[i] = max(
              arr[i] + dp[i-2],
              dp[i-1]
           )
```

---

# 🧠 HOW TO THINK

```text
dp[i]
=
maximum loot till index i
```

---

# 🌳 COMPLETE DRY RUN

```text
arr = [1,2,3,1]
```

---

# CASE 1 → Ignore First

```text
temp1 = [2,3,1]
```

---

# INITIALIZATION

```text
dp[0] = 2
```

DP:

| Index | 0 | 1 | 2 |
|---|---|---|---|
| dp | 2 | 0 | 0 |

---

# i = 1

## PICK

```text
pick = 3
```

---

## NOT PICK

```text
not_pick = 2
```

---

## STORE

```text
dp[1] = 3
```

DP:

| Index | 0 | 1 | 2 |
|---|---|---|---|
| dp | 2 | 3 | 0 |

---

# i = 2

## PICK

```text
pick = 1 + dp[0]
     = 3
```

---

## NOT PICK

```text
not_pick = dp[1]
         = 3
```

---

## STORE

```text
dp[2] = 3
```

---

# CASE 1 ANSWER

```text
3
```

---

# CASE 2 → Ignore Last

```text
temp2 = [1,2,3]
```

---

# FINAL ANSWER

```text
4
```

---

# ✅ TABULATION CODE

```cpp
class Solution {
public:

    int solve(vector<int>& nums) {

        int n = nums.size();

        vector<int> dp(n, 0);

        // Base Case
        dp[0] = nums[0];

        for(int i = 1; i < n; i++) {

            // Pick Current House
            int pick = nums[i];

            if(i > 1)
                pick += dp[i - 2];

            // Skip Current House
            int not_pick = dp[i - 1];

            // Store Maximum
            dp[i] = max(pick, not_pick);
        }

        return dp[n - 1];
    }

    int rob(vector<int>& arr) {

        int n = arr.size();

        if(n == 1)
            return arr[0];

        vector<int> temp1;
        vector<int> temp2;

        // Ignore First
        for(int i = 1; i < n; i++) {
            temp1.push_back(arr[i]);
        }

        // Ignore Last
        for(int i = 0; i < n - 1; i++) {
            temp2.push_back(arr[i]);
        }

        int case1 = solve(temp1);

        int case2 = solve(temp2);

        return max(case1, case2);
    }
};
```

---

# ⏱️ TC

```text
O(N)
```

---

# 🗂️ SC

```text
O(N)
```

---

# ==================================================
# 4️⃣ SPACE OPTIMIZATION
# ==================================================

# 🧠 MAIN OBSERVATION

In tabulation:

```text
dp[i]
depends only on:

dp[i-1]
dp[i-2]
```

So full DP array is unnecessary.

---

# 📌 VARIABLES

```text
prev  -> dp[i-1]
prev2 -> dp[i-2]
```

---

# 🧠 FORMULA

```text
curr = max(
              arr[i] + prev2,
              prev
          )
```

---

# 🌳 COMPLETE DRY RUN

```text
arr = [1,2,3]
```

---

# INITIAL

```text
prev = 1
prev2 = 0
```

---

# i = 1

## PICK

```text
pick = 2
```

---

## NOT PICK

```text
not_pick = 1
```

---

## CURRENT

```text
curr = 2
```

---

## UPDATE

```text
prev2 = 1
prev = 2
```

---

# i = 2

## PICK

```text
pick = 3 + 1
     = 4
```

---

## NOT PICK

```text
not_pick = 2
```

---

## CURRENT

```text
curr = 4
```

---

# FINAL ANSWER

```text
4
```

---

# ✅ SPACE OPTIMIZED CODE

```cpp
class Solution {
public:

    int solve(vector<int>& nums) {

        int n = nums.size();

        int prev = nums[0];
        int prev2 = 0;

        for(int i = 1; i < n; i++) {

            // Pick Current House
            int pick = nums[i];

            if(i > 1)
                pick += prev2;

            // Skip Current House
            int not_pick = prev;

            // Current Maximum
            int curr = max(pick, not_pick);

            // Shift Variables
            prev2 = prev;
            prev = curr;
        }

        return prev;
    }

    int rob(vector<int>& arr) {

        int n = arr.size();

        if(n == 1)
            return arr[0];

        vector<int> temp1;
        vector<int> temp2;

        // Ignore First
        for(int i = 1; i < n; i++) {
            temp1.push_back(arr[i]);
        }

        // Ignore Last
        for(int i = 0; i < n - 1; i++) {
            temp2.push_back(arr[i]);
        }

        int case1 = solve(temp1);

        int case2 = solve(temp2);

        return max(case1, case2);
    }
};
```

---

# ⏱️ TC

```text
O(N)
```

---

# 🗂️ SC

```text
O(1)
```

---

# 🎯 HOW TO EXPLAIN IN INTERVIEW

```text
This is House Robber I with one extra condition:
first and last houses are adjacent.

So we cannot take both together.

Hence we divide problem into two linear subproblems:

1. Ignore first house
2. Ignore last house

Now both become simple linear
House Robber problems.

At every index:
1. Pick current house
   → move to i-2

2. Skip current house
   → move to i-1

Recurrence:
f(i) = max(
              arr[i] + f(i-2),
              f(i-1)
           )

Then optimize:
Recursion
→ Memoization
→ Tabulation
→ Space Optimization
```

---

# 🔥 MOST IMPORTANT LINES

## CASE SPLIT

```cpp
int case1 = solve(n - 2, 0, arr);

int case2 = solve(n - 1, 1, arr);
```

---

## PICK

```cpp
pick = arr[i] + dp[i-2];
```

---

## NOT PICK

```cpp
not_pick = dp[i-1];
```

---

## SPACE OPTIMIZATION SHIFT

```cpp
prev2 = prev;
prev = curr;
```

---

# 🧠 INSTANT RECOGNITION TRICK

Whenever you see:

```text
1. Circular Array
2. Cannot take adjacent elements
3. Maximize sum/profit
```

Immediately think:

```text
Convert circular problem
into
two linear DP problems
```
