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

# 🔁 Recurrence Relation

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

## 💡 Why is f(0) = 1 ?

Because:

```text
Doing nothing is also one valid way
```

This is a very common interview confusion.

---

<br>

# 1️⃣ Recursion (Brute Force)

---

## 💡 Intuition

Ask:

> “How could I reach stair n?”

Two possibilities:
- from `n-1`
- from `n-2`

So recursively calculate both.

---

## 💻 Code

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

## 🌳 Recursive Tree

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

## ⚠️ Problem

Repeated states:
- `f(2)`

This creates overlapping subproblems.

---

## 🌳 Dry Run

```text
f(4)
= f(3) + f(2)

f(3)
= f(2) + f(1)

f(2)
= f(1) + f(0)
```

Base Cases:

```text
f(1) = 1
f(0) = 1
```

Final Answer:

```text
f(4) = 5
```

---

## ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(2^n) |
| Space Complexity | O(n) |

---

<br>

# 2️⃣ Memoization (Top Down DP)

---

## 💡 Key Idea

Store already computed answers.

```text
dp[n] = number of ways to reach stair n
```

Before solving:
- check if answer already exists

---

## 🎯 Important Interview Line

> "We are caching overlapping subproblems."

---

## 💻 Code

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

## 📌 Important Memoization Line

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

## 🌳 Dry Run

### Initial DP Array

```text
[-1,-1,-1,-1,-1]
```

---

### After Calculating dp[2]

```text
[-1,-1,2,-1,-1]
```

---

### After Calculating dp[3]

```text
[-1,-1,2,3,-1]
```

---

### Final DP Array

```text
[-1,-1,2,3,5]
```

---

## ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

<br>

# 3️⃣ Tabulation (Bottom Up DP)

---

## 💡 Key Idea

Instead of solving:

```text
n → n-1 → n-2
```

build answers from:

```text
0 → 1 → 2 → 3 → n
```

---

## 📌 DP State

```text
dp[i] = number of ways to reach stair i
```

---

## 💻 Code

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

## 🌳 Dry Run

### Initial

```text
[1,1,_,_,_]
```

---

### Iteration

```text
i = 2
dp[2] = 1 + 1 = 2
```

Array:

```text
[1,1,2,_,_]
```

---

```text
i = 3
dp[3] = 2 + 1 = 3
```

Array:

```text
[1,1,2,3,_]
```

---

```text
i = 4
dp[4] = 3 + 2 = 5
```

Final:

```text
[1,1,2,3,5]
```

Answer:

```text
5
```

---

## ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

<br>

# 4️⃣ Space Optimization

---

## 💡 Important Observation

We only need:
- previous value
- second previous value

Entire DP array is unnecessary.

---

## 💻 Code

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

## 🌳 Dry Run

### Initial

```text
prev2 = 1
prev1 = 1
```

---

### Iteration

```text
i = 2

curr = 1 + 1 = 2

prev2 = 1
prev1 = 2
```

---

```text
i = 3

curr = 2 + 1 = 3

prev2 = 2
prev1 = 3
```

---

```text
i = 4

curr = 3 + 2 = 5

prev2 = 3
prev1 = 5
```

Return:

```text
5
```

---

## ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(1) |

---

<br>

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

## ❓ Why DP?

Because recursion repeats states.

Example:

```text
f(2) gets calculated multiple times
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

# 🧩 Golden Memory Trick

```text
Think about LAST MOVE

Current answer
=
sum of all valid previous states
```

---

# 🔥 DP Evolution

```text
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
```
