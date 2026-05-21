# 🚀 Fibonacci Sequence

> Classic Dynamic Programming problem based on overlapping subproblems.

---

# 📌 Pattern

| Category | Type |
|----------|------|
| Pattern | Dynamic Programming |
| Difficulty | Easy |
| Concepts | Recursion, Memoization, Tabulation, Space Optimization |

---

# 🧠 Why DP?

- Same recursive calls repeat
- Problem has overlapping subproblems
- Current answer depends on previous answers

---

# 🔁 Recurrence Relation

```text
F(n) = F(n-1) + F(n-2)
```

---

# 🧩 DP State

```text
dp[i] = fibonacci of i
```

---

# 🌟 Golden DP Flow

```text
Recursion
   ↓
Repeated Calls
   ↓
Memoization
   ↓
Tabulation
   ↓
Space Optimization
```

---

<br>

# 1️⃣ Recursion (Brute Force)

---

# 💡 Intuition

To calculate:

```text
fib(n)
```

we need:

```text
fib(n-1)
fib(n-2)
```

Directly follow the recurrence relation.

---

# 💻 Code

```cpp
class Solution {
public:

    int fib(int n) {

        // Base Case
        if(n <= 1)
            return n;

        return fib(n-1) + fib(n-2);
    }
};
```

---

# 🌳 Recursive Tree

```text
fib(5)

├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   └── fib(1)
│   └── fib(2)
└── fib(3)
```

---

# ⚠️ Problem

Repeated calls:

```text
fib(3)
fib(2)
```

This creates repeated computation.

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(2^n) |
| Space Complexity | O(n) |

---

# ❌ Why Recursion is Bad Here

Too many repeated computations.

Example:

```text
fib(40)
```

becomes extremely slow.

---

<br>

# 2️⃣ Memoization (Top Down DP)

---

# 💡 Key Idea

Store already computed answers.

```text
dp[n] = fib(n)
```

Before solving:

```text
Check if answer already exists
```

---

# 🎯 Important Interview Line

> "We are caching overlapping subproblems."

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int n, vector<int>& dp) {

        // Base Case
        if(n <= 1)
            return n;

        // Already Computed
        if(dp[n] != -1)
            return dp[n];

        // Store Answer
        dp[n] = solve(n-1, dp) + solve(n-2, dp);

        return dp[n];
    }

    int fib(int n) {

        vector<int> dp(n+1, -1);

        return solve(n, dp);
    }
};
```

---

# ❓ Important Doubt

## Why not store dp[0] and dp[1]?

We CAN.

Example:

```cpp
dp[0] = 0;
dp[1] = 1;
```

But many people directly return:

```cpp
if(n <= 1)
    return n;
```

Both are correct.

---

# 🌳 Memoization Dry Run

## Initial DP Array

```text
[-1,-1,-1,-1,-1,-1]
```

---

## Compute fib(2)

```text
fib(1) + fib(0)
= 1 + 0
= 1
```

Store:

```text
dp[2] = 1
```

---

## Compute fib(3)

```text
fib(2) + fib(1)
= 1 + 1
= 2
```

Store:

```text
dp[3] = 2
```

---

## Compute fib(4)

```text
fib(3) + fib(2)
= 2 + 1
= 3
```

Store:

```text
dp[4] = 3
```

---

## Compute fib(5)

```text
fib(4) + fib(3)
= 3 + 2
= 5
```

Store:

```text
dp[5] = 5
```

---

# 📦 Final DP Array

```text
[-1,-1,1,2,3,5]
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

# 📦 Why Space Complexity is O(n)

Memoization uses:

- DP array
- Recursion stack

```text
DP Array → O(n)
Recursion Stack → O(n)
```

Total:

```text
O(n)
```

---

<br>

# 3️⃣ Tabulation (Bottom Up DP)

---

# 💡 Key Idea

Instead of solving:

```text
n → n-1 → n-2
```

build answers from:

```text
0 → 1 → 2 → 3 → n
```

---

# 📌 DP State

```text
dp[i] = fibonacci of i
```

---

# 💻 Code

```cpp
class Solution {
public:

    int fib(int n) {

        if(n <= 1)
            return n;

        vector<int> dp(n+1);

        dp[0] = 0;
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

## Initial DP Array

```text
[0,1,_,_,_,_]
```

---

## i = 2

```text
dp[2] = dp[1] + dp[0]
      = 1 + 0
      = 1
```

```text
[0,1,1,_,_,_]
```

---

## i = 3

```text
dp[3] = dp[2] + dp[1]
      = 1 + 1
      = 2
```

```text
[0,1,1,2,_,_]
```

---

## i = 4

```text
dp[4] = dp[3] + dp[2]
      = 2 + 1
      = 3
```

```text
[0,1,1,2,3,_]
```

---

## i = 5

```text
dp[5] = dp[4] + dp[3]
      = 3 + 2
      = 5
```

Final:

```text
[0,1,1,2,3,5]
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(n) |

---

<br>

# 4️⃣ Space Optimization

---

# 💡 Important Observation

To calculate current Fibonacci,
we only need:

```text
previous value
second previous value
```

Entire DP array is unnecessary.

---

# ❓ Why This Works

Formula only depends on:

```text
F(n) = F(n-1) + F(n-2)
```

So storing all states is unnecessary.

---

# 💻 Code

```cpp
class Solution {
public:

    int fib(int n) {

        if(n <= 1)
            return n;

        int prev2 = 0;
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

# 📚 Meaning of Variables

| Variable | Meaning |
|---|---|
| prev2 | fib(i-2) |
| prev1 | fib(i-1) |
| curr | fib(i) |

---

# ❓ Very Important Doubt

## Why no curr[i] ?

Because:

```text
curr
```

is NOT an array.

It is just one variable storing current answer.

---

# 🌳 Space Optimization Dry Run

## Initial

```text
prev2 = 0
prev1 = 1
```

---

## i = 2

```text
curr = 1
```

Update:

```text
prev2 = 1
prev1 = 1
```

---

## i = 3

```text
curr = 2
```

Update:

```text
prev2 = 1
prev1 = 2
```

---

## i = 4

```text
curr = 3
```

Update:

```text
prev2 = 2
prev1 = 3
```

---

## i = 5

```text
curr = 5
```

Update:

```text
prev2 = 3
prev1 = 5
```

Answer:

```text
5
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(1) |

---

# 📊 Final Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimization | O(n) | O(1) |

---

# 🎤 Interview Flow

## Step 1

Say:

```text
"This problem has overlapping subproblems."
```

---

## Step 2

Write recursion first.

Shows:
- recurrence understanding
- problem clarity

---

## Step 3

Point out repeated calls.

Example:

```text
fib(3)
fib(2)
```

repeat many times.

---

## Step 4

Say:

```text
"We can cache computed states using DP."
```

Move to memoization.

---

## Step 5

Then say:

```text
"Since recursion stack still exists,
we can convert this into iterative tabulation."
```

---

## Step 6

Finally say:

```text
"We only need previous two states,
so we can optimize space."
```

Interviewers LOVE this progression.

---

# ⚠️ Edge Cases

```text
n = 0
n = 1
very large n
```

---

# ❌ Common Mistakes

- Forgetting base cases
- Mixing memoization and tabulation
- Forgetting DP state meaning
- Confusing curr with curr[i]

---

# 🧠 Universal DP Thinking

```text
1. Define the STATE
2. Try all possible CHOICES
3. Combine answers
4. Store the result
```

---

# 🌟 Universal DP Template

```text
1. Define State

2. Try all choices from that state

3. Combine answers:
   - SUM for count problems
   - MIN/MAX for optimization problems

4. Write base case

5. Memoize
```

---

# 🎯 Golden Interview Questions

Whenever stuck in DP, ask:

---

## Question 1

```text
What is my state?
```

---

## Question 2

```text
What choices can I make from this state?
```

---

## Question 3

```text
Am I:
- counting all ways?
OR
- finding the best way?
```

---

# 🧩 Golden Memory Trick

```text
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
```

This is Dynamic Programming in one sentence.
