# 🚀 Ways to Reach the N-th Stair

> Classic Dynamic Programming problem based on the Fibonacci Pattern.

---

# 📌 Pattern

| Category | Type |
|----------|------|
| Pattern | Dynamic Programming |
| Difficulty | Easy |
| Concepts | Recursion, Memoization, Tabulation, Space Optimization |

---

# 🧠 Why This Pattern?

Current stair depends on:
- previous stair
- two stairs behind

This creates the recurrence:

```text
f(n) = f(n-1) + f(n-2)
```

Classic Fibonacci-style DP problem.

---

# ❓ Problem Understanding

There are `n` stairs.

A person can climb:
- 1 stair
- 2 stairs

We need to find:

> Total number of distinct ways to reach stair `n`

Order matters.

---

## 📌 Example

For:

```text
n = 3
```

Ways:

```text
1 + 1 + 1
1 + 2
2 + 1
```

Answer:

```text
3
```

---

# ⚡ DP Shortcut Thinking

Whenever question says:
- count ways
- paths
- sequences

Think:

> “How can I reach the current position?”

Break the problem using:

```text
LAST MOVE
```

---

# 🔁 Core Recurrence Relation

To reach stair `n`:
- either come from `n-1`
- or come from `n-2`

Hence:

```text
f(n) = f(n-1) + f(n-2)
```

---

# 📌 State Definition

```text
f(n) = number of ways to reach stair n
```

---

# 🎯 Base Cases

```text
f(0) = 1
f(1) = 1
```

---

## 💡 Why is `f(0) = 1` ?

Because:

```text
Doing nothing is also one valid way
```

This is one of the most common interview confusions.

---

# 🧠 How To Derive The Recurrence

This is the MOST important DP skill.

Ask:

> “How could I reach stair n?”

To stand at stair `n`:

- last jump could come from `n-1`
- last jump could come from `n-2`

So total ways become:

```text
ways reaching (n-1)
+
ways reaching (n-2)
```

Hence:

```text
f(n) = f(n-1) + f(n-2)
```

---

# 🔥 Golden DP Rule

For counting problems:

```text
Current answer
=
sum of all valid previous states
```

---

<br>

# 1️⃣ Recursion (Brute Force)

---

# 💡 Recursion Idea

Ask:

> “How could I reach stair n?”

Two possibilities:
- from `n-1`
- from `n-2`

So recursively calculate both.

---

# 💻 Code

```cpp
class Solution {
public:

    int countWays(int n) {

        // Base Cases
        if(n == 0 || n == 1)
            return 1;

        return countWays(n-1) + countWays(n-2);
    }
};
```

---

# 🌳 Recursive Tree

```text
f(4)

├── f(3)
│   ├── f(2)
│   │   ├── f(1)
│   │   └── f(0)
│   │
│   └── f(1)
│
└── f(2)
    ├── f(1)
    └── f(0)
```

---

# ⚠️ Problem With Recursion

Repeated states:
- `f(2)`

This creates:

```text
OVERLAPPING SUBPROBLEMS
```

Which makes recursion slow.

---

# 🌳 Dry Run

Input:

```text
n = 4
```

Calls:

```text
f(4)
= f(3) + f(2)
```

Now:

```text
f(3)
= f(2) + f(1)
```

Now:

```text
f(2)
= f(1) + f(0)
```

Base Cases:

```text
f(1) = 1
f(0) = 1
```

Now:

```text
f(2) = 2
f(3) = 3
f(4) = 5
```

Final Answer:

```text
5
```

---

# ⏱️ Complexity Analysis

## 🕒 Time Complexity

```text
O(2^n)
```

---

## 💾 Space Complexity

```text
O(n)
```

(recursion stack)

---

<br>

# 2️⃣ Memoization (Top Down DP)

---

# 💡 Memoization Idea

Store already computed answers.

```text
dp[n] = number of ways to reach stair n
```

Before solving:
- check if answer already exists

If yes:
- directly return stored answer

---

# 🎯 Important Interview Line

> "We are caching overlapping subproblems."

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int n, vector<int>& dp) {

        // Base Cases
        if(n == 0 || n == 1)
            return 1;

        // Already Computed
        if(dp[n] != -1)
            return dp[n];

        // Store Answer
        dp[n] = solve(n-1, dp) + solve(n-2, dp);

        return dp[n];
    }

    int countWays(int n) {

        vector<int> dp(n+1, -1);

        return solve(n, dp);
    }
};
```

---

# 📌 Most Important Memoization Line

```cpp
if(dp[n] != -1)
    return dp[n];
```

Meaning:

```text
If already solved before,
reuse stored answer
```

This avoids repeated recursion.

---

# 🌳 Memoization Dry Run

Input:

```text
n = 4
```

---

## Initial DP Array

```text
[-1,-1,-1,-1,-1]
```

---

## Calculate `dp[2]`

```text
dp[2] = solve(1) + solve(0)
       = 1 + 1
       = 2
```

Array:

```text
[-1,-1,2,-1,-1]
```

---

## Calculate `dp[3]`

```text
dp[3] = solve(2) + solve(1)
       = 2 + 1
       = 3
```

Array:

```text
[-1,-1,2,3,-1]
```

---

## Now `solve(2)` Comes Again

But:

```text
dp[2] already exists
```

So:

```cpp
return dp[2];
```

No recursion happens.

This is the optimization.

---

## Final

```text
dp[4] = 5
```

Final DP Array:

```text
[-1,-1,2,3,5]
```

---

# ⏱️ Complexity Analysis

## 🕒 Time Complexity

```text
O(n)
```

---

## 💾 Space Complexity

```text
O(n)
```

(DP array + recursion stack)

---

<br>

# 3️⃣ Tabulation (Bottom Up DP)

---

# 💡 Tabulation Idea

Remove recursion completely.

Instead of solving:

```text
n → n-1 → n-2
```

Build answers from:

```text
0 → 1 → 2 → 3 → n
```

---

# 📌 DP State

```text
dp[i] = number of ways to reach stair i
```

---

# 💻 Code

```cpp
class Solution {
public:

    int countWays(int n) {

        vector<int> dp(n+1);

        dp[0] = 1;
        dp[1] = 1;

        for(int i=2; i<=n; i++) {

            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
};
```

---

# 🌳 Tabulation Dry Run

Input:

```text
n = 4
```

---

## Initial DP Array

```text
[1,1,_,_,_]
```

---

## Iteration 1

```text
i = 2

dp[2] = dp[1] + dp[0]
      = 1 + 1
      = 2
```

Array:

```text
[1,1,2,_,_]
```

---

## Iteration 2

```text
i = 3

dp[3] = dp[2] + dp[1]
      = 2 + 1
      = 3
```

Array:

```text
[1,1,2,3,_]
```

---

## Iteration 3

```text
i = 4

dp[4] = dp[3] + dp[2]
      = 3 + 2
      = 5
```

Final Array:

```text
[1,1,2,3,5]
```

Answer:

```text
5
```

---

# ⏱️ Complexity Analysis

## 🕒 Time Complexity

```text
O(n)
```

---

## 💾 Space Complexity

```text
O(n)
```

---

<br>

# 4️⃣ Space Optimization

---

# 💡 Important Observation

We only need:
- previous value
- second previous value

Entire DP array is unnecessary.

---

# 📌 Variable Meaning

```text
prev1 = dp[i-1]
prev2 = dp[i-2]
```

---

# 💻 Code

```cpp
class Solution {
public:

    int countWays(int n) {

        if(n == 0 || n == 1)
            return 1;

        int prev2 = 1;
        int prev1 = 1;

        for(int i=2; i<=n; i++) {

            int curr = prev1 + prev2;

            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
};
```

---

# 🌳 Space Optimization Dry Run

Input:

```text
n = 4
```

---

## Initial

```text
prev2 = 1
prev1 = 1
```

---

## Iteration 1

```text
i = 2

curr = 1 + 1 = 2
```

Update:

```text
prev2 = 1
prev1 = 2
```

---

## Iteration 2

```text
i = 3

curr = 2 + 1 = 3
```

Update:

```text
prev2 = 2
prev1 = 3
```

---

## Iteration 3

```text
i = 4

curr = 3 + 2 = 5
```

Update:

```text
prev2 = 3
prev1 = 5
```

Return:

```text
5
```

---

# ⏱️ Complexity Analysis

## 🕒 Time Complexity

```text
O(n)
```

---

## 💾 Space Complexity

```text
O(1)
```

---

# 🎤 Interview Flow

```text
Recursion
   ↓
Repeated States
   ↓
Memoization
   ↓
Tabulation
   ↓
Space Optimization
```

---

# 🧠 Interview Explanation

If interviewer asks:

> “How did you derive the recurrence?”

Say:

```text
To reach stair n,
the last jump could either come from n-1
or from n-2.

Therefore,

ways(n)
=
ways(n-1) + ways(n-2)
```

---

# ⚠️ Important Interview Points

---

## ❓ Why DP?

Because recursion repeats states.

Example:

```text
f(2) gets calculated multiple times
```

---

## ❓ Why Memoization?

```text
Stores already computed answers
```

---

## ❓ Why Tabulation?

```text
Removes recursion stack
```

---

## ❓ Why Space Optimization?

```text
Only previous 2 states are needed
```

---

# 📊 Complete Complexity Table

| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimization | O(n) | O(1) |

---

# ⚠️ Edge Cases

- `n = 0`
- `n = 1`
- very large `n`

---

# ❌ Common Mistakes

- Forgetting base cases
- Using `f(0) = 0`
- Forgetting DP state meaning
- Mixing memoization and tabulation
- Updating variables in wrong order

---

# 🧩 Golden Memory Tricks

## 🔥 Recurrence Building

```text
Think about LAST MOVE
```

---

## 🔥 Counting Problems

```text
Current answer
=
sum of all valid previous states
```

---

## 🔥 Memoization

```text
Recursion + DP storage
```

---

## 🔥 Tabulation

```text
Build answers from small → large
```

---

## 🔥 Space Optimization

```text
If only few previous states are needed,
replace array with variables
```

---

# 🚀 DP Evolution Summary

```text
Recursion repeats states
        ↓
Store answers (Memoization)
        ↓
Remove recursion (Tabulation)
        ↓
Remove extra space (Space Optimization)
```
