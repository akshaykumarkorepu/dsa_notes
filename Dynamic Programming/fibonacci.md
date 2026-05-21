# 🚀 Fibonacci Sequence

> Classic Dynamic Programming problem based on overlapping subproblems.

---

# 📌 Pattern

| Category | Type |
|----------|------|
| Pattern | Dynamic Programming |
| Difficulty | Easy |
| Concepts | Recursion, Memoization, Tabulation |

---

# 🧠 Why DP?

- Same recursive calls repeat
- Problem has overlapping subproblems
- Current answer depends on previous answers

---

# 🔁 Recurrence Relation

F(n) = F(n-1) + F(n-2)

---

<br>

# 1️⃣ Recursion (Brute Force)

---

## 💡 Intuition

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

## 💻 Code

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

## 🌳 Recursive Tree

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

## ⚠️ Problem

Repeated calls:
- fib(3)
- fib(2)

This creates repeated computation.

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
dp[n] = fib(n)
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

## 🌳 Dry Run

### Initial DP Array

```text
[-1,-1,-1,-1,-1,-1]
```

### Final DP Array

```text
[-1,-1,1,2,3,5]
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
dp[i] = fibonacci of i
```

---

## 💻 Code

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

## 🌳 Dry Run

### Initial

```text
[0,1,_,_,_,_]
```

### Final

```text
[0,1,1,2,3,5]
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
Repeated Calls
   ↓
Memoization
   ↓
Tabulation
   ↓
Space Optimization
```

---

# ⚠️ Edge Cases

- n = 0
- n = 1
- very large n

---

# ❌ Common Mistakes

- Forgetting base cases
- Mixing memoization and tabulation
- Forgetting DP state meaning
- Confusing curr with curr[i]

---

# 🧩 Golden Memory Trick

```text
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
```
