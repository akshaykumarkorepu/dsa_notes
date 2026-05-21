# 🚀 Stickler Thief

> Classic Dynamic Programming problem based on the **Pick / Not Pick Pattern**

---

# 📌 Problem Statement

A thief wants to loot houses arranged in a line.

Constraint:

```text
Cannot loot two consecutive houses
```

Find the maximum money he can loot.

---

# 📌 Pattern

| Category | Type |
|---|---|
| Pattern | Dynamic Programming |
| Sub Pattern | Pick / Not Pick |
| DP Type | 1D DP |

---

# 🧠 WHY THIS PATTERN?

At every house we have only 2 choices:

```text
1. Pick current house
2. Skip current house
```

This becomes a classic:

```text
Take / Not Take DP
```

---

# 🎯 CORE IDEA

For every index:

```text
Maximum Loot
=
max(
    pick current house,
    skip current house
)
```

---

# 🧠 DP SHORTCUT THINKING

# STEP 1 → DEFINE STATE

```text
f(i)
=
maximum money we can loot till index i
```

---

# STEP 2 → FIND CHOICES

```text
1. Pick current house
2. Do not pick current house
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

# STEP 4 → BASE CASES

```cpp
if(i == 0) return arr[0];

if(i < 0) return 0;
```

---

# 🧠 HOW TO IDENTIFY THIS QUESTION IN INTERVIEW

Whenever you see:

```text
1. Cannot take adjacent elements
2. Maximize sum/profit
3. Pick or skip choice
```

Think:

```text
PICK / NOT PICK DP
```

---

# 🚨 WHY PEOPLE GET STUCK

```text
1. Forgetting why we move to i-2

2. Forgetting:
pick current
means adjacent cannot be picked

3. Confusing current answer
with global maximum

4. Forgetting recurrence relation
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

# 🌳 RECURSION TREE

```text
solve(3)

├── pick
│   └── arr[3] + solve(1)
│
└── not_pick
    └── solve(2)
```

---

# 🌳 DRY RUN

```text
arr = [5, 3, 4]
```

We call:

```text
solve(2)
```

---

## OPTION 1 → PICK

```text
4 + solve(0)

= 4 + 5

= 9
```

---

## OPTION 2 → NOT PICK

```text
solve(1)

max(
    3 + solve(-1),
    solve(0)
)

max(3, 5)

= 5
```

---

## FINAL

```text
max(9, 5)
=
9
```

---

# ✅ RECURSION CODE

```cpp
class Solution {
  public:
  
    int solve(int ind, vector<int> &arr) {
        
        // Base Case
        if(ind == 0)
            return arr[ind];
        
        if(ind < 0)
            return 0;
        
        // Pick Current House
        int pick = arr[ind] + solve(ind - 2, arr);
        
        // Skip Current House
        int not_pick = solve(ind - 1, arr);
        
        // Return Maximum
        return max(pick, not_pick);
    }
    
    int findMaxSum(vector<int>& arr) {
        
        int n = arr.size();
        
        return solve(n - 1, arr);
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

(recursion stack)

---

# 🚨 WHY TC IS EXPONENTIAL?

```text
Every index generates 2 recursive calls
```

Same states repeat again and again.

---

# ==================================================
# 2️⃣ MEMOIZATION
# ==================================================

# 🧠 IDEA

In recursion:

```text
Same subproblems repeat
```

Example:

```text
solve(3) calls solve(1)

solve(2) also calls solve(1)
```

So store answers in DP array.

---

# 📌 MEMOIZATION STEPS

```text
1. Create dp array

2. Before solving:
   check if answer already exists

3. Store answer before returning
```

---

# 🌳 DRY RUN IDEA

Suppose:

```text
solve(2)
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
  
    int solve(int ind, vector<int> &arr, vector<int> &dp) {
        
        // Base Case
        if(ind == 0)
            return arr[ind];
        
        if(ind < 0)
            return 0;
        
        // DP Check
        if(dp[ind] != -1)
            return dp[ind];
        
        // Pick
        int pick = arr[ind] + solve(ind - 2, arr, dp);
        
        // Not Pick
        int not_pick = solve(ind - 1, arr, dp);
        
        // Store and Return
        return dp[ind] = max(pick, not_pick);
    }
    
    int findMaxSum(vector<int>& arr) {
        
        int n = arr.size();
        
        vector<int> dp(n, -1);
        
        return solve(n - 1, arr, dp);
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

# 📌 WHY ONLY ONE BASE CASE?

In recursion:

```cpp
if(i < 0)
```

was needed because recursion could go negative.

But in tabulation:

```text
Loop never visits negative indices
```

So only:

```cpp
dp[0] = arr[0];
```

is enough.

---

# 🌳 COMPLETE DRY RUN

```text
arr = [5, 3, 4, 11, 2]
```

---

# INITIALIZATION

```cpp
dp[0] = 5
```

DP:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp | 5 | 0 | 0 | 0 | 0 |

---

# i = 1

## PICK

```text
pick = 3
```

Why not `3 + dp[-1]`?

Because:

```cpp
if(i > 1)
```

fails.

---

## NOT PICK

```text
not_pick = dp[0]
          = 5
```

---

## STORE

```text
dp[1] = max(3, 5)
      = 5
```

DP:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp | 5 | 5 | 0 | 0 | 0 |

---

# i = 2

## PICK

```text
pick = 4 + dp[0]
     = 9
```

---

## NOT PICK

```text
not_pick = dp[1]
         = 5
```

---

## STORE

```text
dp[2] = 9
```

DP:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp | 5 | 5 | 9 | 0 | 0 |

---

# i = 3

## PICK

```text
pick = 11 + dp[1]
     = 16
```

---

## NOT PICK

```text
not_pick = dp[2]
         = 9
```

---

## STORE

```text
dp[3] = 16
```

DP:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp | 5 | 5 | 9 | 16 | 0 |

---

# i = 4

## PICK

```text
pick = 2 + dp[2]
     = 11
```

---

## NOT PICK

```text
not_pick = dp[3]
         = 16
```

---

## STORE

```text
dp[4] = 16
```

Final DP:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp | 5 | 5 | 9 | 16 | 16 |

---

# ✅ TABULATION CODE

```cpp
class Solution {
  public:
  
    int findMaxSum(vector<int>& arr) {
        
        int n = arr.size();
        
        vector<int> dp(n, 0);
        
        // Base Case
        dp[0] = arr[0];
        
        for(int i = 1; i < n; i++) {
            
            int pick = arr[i];
            
            if(i > 1)
                pick += dp[i - 2];
            
            int not_pick = dp[i - 1];
            
            dp[i] = max(pick, not_pick);
        }
        
        return dp[n - 1];
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
arr = [5, 3, 4, 11, 2]
```

---

# INITIAL

```text
prev = 5
prev2 = 0
```

---

# i = 1

## PICK

```text
pick = 3
```

---

## NOT PICK

```text
not_pick = 5
```

---

## CURRENT

```text
curr = 5
```

---

## UPDATE

```text
prev2 = 5
prev = 5
```

---

# i = 2

## PICK

```text
pick = 4 + 5
     = 9
```

---

## NOT PICK

```text
not_pick = 5
```

---

## CURRENT

```text
curr = 9
```

---

## UPDATE

```text
prev2 = 5
prev = 9
```

---

# i = 3

## PICK

```text
pick = 11 + 5
     = 16
```

---

## NOT PICK

```text
not_pick = 9
```

---

## CURRENT

```text
curr = 16
```

---

## UPDATE

```text
prev2 = 9
prev = 16
```

---

# i = 4

## PICK

```text
pick = 2 + 9
     = 11
```

---

## NOT PICK

```text
not_pick = 16
```

---

## CURRENT

```text
curr = 16
```

---

# ✅ FINAL ANSWER

```text
16
```

---

# ✅ SPACE OPTIMIZED CODE

```cpp
class Solution {
  public:
  
    int findMaxSum(vector<int>& arr) {
        
        int n = arr.size();
        
        int prev = arr[0];
        int prev2 = 0;
        
        for(int i = 1; i < n; i++) {
            
            int pick = arr[i];
            
            if(i > 1)
                pick += prev2;
            
            int not_pick = prev;
            
            int curr = max(pick, not_pick);
            
            prev2 = prev;
            prev = curr;
        }
        
        return prev;
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
This is a classic Pick / Not Pick DP problem.

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

## PICK

```cpp
int pick = arr[i] + dp[i-2];
```

---

## NOT PICK

```cpp
int not_pick = dp[i-1];
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
1. Cannot take adjacent elements
2. Maximize sum/profit
3. Pick or skip choice
```

Immediately think:

```text
PICK / NOT PICK DP
```
