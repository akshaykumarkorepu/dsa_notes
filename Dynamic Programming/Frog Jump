# 🐸 Frog Jump

> Classic Dynamic Programming problem based on minimum cost path and Fibonacci-style transitions.

---

# 📌 Pattern

| Category | Type |
|----------|------|
| Pattern | Dynamic Programming |
| Difficulty | Easy-Medium |
| Concepts | Recursion, Memoization, Tabulation, Space Optimization |

---

# 🧠 Why This Pattern?

Current stair depends on:
- previous stair
- two stairs behind

We try:
- jump from `i-1`
- jump from `i-2`

and take minimum cost.

This creates the recurrence:

```text
f(i) =
min(
    f(i-1) + abs(height[i] - height[i-1]),

    f(i-2) + abs(height[i] - height[i-2])
)
```

Classic Fibonacci-style DP problem.

---

# ❓ Problem Understanding

We are given:

```text
height[]
```

where:

```text
height[i]
```

represents the height of the `i-th` stair.

A frog starts from:

```text
index 0
```

Goal:

```text
Reach index n-1
```

Allowed jumps:
- 1 step
- 2 steps

Jump cost:

```text
abs(height[i] - height[j])
```

We need:

> Minimum total energy required to reach the last stair.

---

# 📌 Example

Input:

```text
height = [20,30,40,20]
```

Possible path:

```text
0 -> 1 -> 3
```

Cost:

```text
|30-20| + |20-30|
=
10 + 10
=
20
```

Answer:

```text
20
```

---

# ⚡ DP Shortcut Thinking

Whenever question says:
- minimum cost
- minimum energy
- jumps
- paths

Think:

```text
“How can I reach the current position?”
```

Break the problem using:

```text
LAST JUMP
```

---

# 🔁 Recurrence Relation

To reach stair `i`:
- either come from `i-1`
- or come from `i-2`

Hence:

```text
f(i) =
min(
    f(i-1) + abs(height[i] - height[i-1]),

    f(i-2) + abs(height[i] - height[i-2])
)
```

---

# 📌 State Definition

```text
f(i) = minimum energy required to reach stair i
```

---

# 🎯 Base Case

```text
f(0) = 0
```

Because:

```text
Already standing at stair 0
No energy required
```

---

# 🧠 Core Idea

At every stair:

```text
Try all possible jumps
Take minimum answer
```

---

# ⚠️ Edge Cases

- only 1 stair
- all heights same
- larger jump cheaper than smaller jumps

---

# ❌ Why I Might Forget This Problem

- Forgetting recurrence relation
- Forgetting:
  
```cpp
if(i > 1)
```

- Forgetting this is:
  
```text
minimum path DP
```

---

<br>

# 1️⃣ Recursion (Brute Force)

---

# 💡 Intuition

Ask:

```text
“How could I reach stair i?”
```

Two possibilities:
- from `i-1`
- from `i-2`

So recursively calculate both.

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int ind, vector<int>& height) {

        // Base Case
        if(ind == 0)
            return 0;

        // Jump from ind-1
        int left =
            solve(ind - 1, height)
            + abs(height[ind] - height[ind - 1]);

        // Jump from ind-2
        int right = INT_MAX;

        if(ind > 1) {

            right =
                solve(ind - 2, height)
                + abs(height[ind] - height[ind - 2]);
        }

        return min(left, right);
    }

    int frogJump(vector<int>& height) {

        int n = height.size();

        return solve(n - 1, height);
    }
};
```

---

# 📌 Important Recursion Snippets

## Base Case

```cpp
if(ind == 0)
    return 0;
```

---

## One Step Jump

```cpp
solve(ind-1)
+
abs(height[ind] - height[ind-1])
```

---

## Two Step Jump

```cpp
solve(ind-2)
+
abs(height[ind] - height[ind-2])
```

---

## Final Answer

```cpp
return min(left, right);
```

---

# 🌳 Recursive Tree

```text
solve(3)

├── solve(2)
│   ├── solve(1)
│   │   └── solve(0)
│   │
│   └── solve(0)
│
└── solve(1)
    └── solve(0)
```

---

# ⚠️ Problem

Repeated states:
- `solve(1)`
- `solve(0)`

This creates overlapping subproblems.

---

# 🌳 Dry Run

Input:

```text
height = [20,30,40,20]
```

Need:

```text
solve(3)
```

---

## solve(3)

From stair `2`:

```text
solve(2) + abs(20 - 40)
=
solve(2) + 20
```

From stair `1`:

```text
solve(1) + abs(20 - 30)
=
solve(1) + 10
```

---

## solve(2)

From stair `1`:

```text
solve(1) + abs(40 - 30)
=
solve(1) + 10
```

From stair `0`:

```text
solve(0) + abs(40 - 20)
=
20
```

---

## solve(1)

```text
solve(0) + abs(30 - 20)
=
10
```

---

# 🔙 Backtracking

```text
solve(1) = 10
```

```text
solve(2) = min(20,20)
          = 20
```

```text
solve(3) = min(40,20)
          = 20
```

Final Answer:

```text
20
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(2^N) |
| Space Complexity | O(N) |

---

<br>

# 2️⃣ Memoization (Top Down DP)

---

# 💡 Key Idea

Store already computed answers.

```text
dp[i] = minimum energy to reach stair i
```

Before solving:
- check if answer already exists

---

# 🎯 Important Interview Line

```text
We are caching overlapping subproblems.
```

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int ind,
              vector<int>& height,
              vector<int>& dp) {

        if(ind == 0)
            return 0;

        // Already Computed
        if(dp[ind] != -1)
            return dp[ind];

        int left =
            solve(ind - 1, height, dp)
            + abs(height[ind] - height[ind - 1]);

        int right = INT_MAX;

        if(ind > 1) {

            right =
                solve(ind - 2, height, dp)
                + abs(height[ind] - height[ind - 2]);
        }

        return dp[ind] = min(left, right);
    }

    int frogJump(vector<int>& height) {

        int n = height.size();

        vector<int> dp(n, -1);

        return solve(n - 1, height, dp);
    }
};
```

---

# 📌 Important Memoization Line

```cpp
if(dp[ind] != -1)
    return dp[ind];
```

Meaning:

```text
If already solved before,
reuse stored answer
```

This avoids repeated recursion.

---

# 🌳 Dry Run

### Initial DP Array

```text
[-1,-1,-1,-1]
```

---

### After Solving dp[1]

```text
[-1,10,-1,-1]
```

---

### After Solving dp[2]

```text
[-1,10,20,-1]
```

---

### Final DP Array

```text
[-1,10,20,20]
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(N) |
| Space Complexity | O(N) + O(N) |

---

<br>

# 3️⃣ Tabulation (Bottom Up DP)

---

# 💡 Key Idea

Instead of solving:

```text
n -> n-1 -> n-2
```

build answers from:

```text
0 -> 1 -> 2 -> 3 -> n
```

---

# 📌 DP State

```text
dp[i] = minimum energy required to reach stair i
```

---

# 💻 Code

```cpp
class Solution {
public:

    int frogJump(vector<int>& height) {

        int n = height.size();

        vector<int> dp(n, 0);

        dp[0] = 0;

        for(int i = 1; i < n; i++) {

            int left =
                dp[i - 1]
                + abs(height[i] - height[i - 1]);

            int right = INT_MAX;

            if(i > 1) {

                right =
                    dp[i - 2]
                    + abs(height[i] - height[i - 2]);
            }

            dp[i] = min(left, right);
        }

        return dp[n - 1];
    }
};
```

---

# 🌳 Dry Run

Input:

```text
height = [20,30,40,20]
```

---

# Initial DP

```text
[0,0,0,0]
```

---

# i = 1

```text
left = 0 + abs(30-20)
      = 10
```

2-step jump impossible.

```text
dp[1] = 10
```

DP:

```text
[0,10,0,0]
```

---

# i = 2

```text
left = 10 + abs(40-30)
      = 20
```

```text
right = 0 + abs(40-20)
       = 20
```

```text
dp[2] = min(20,20)
       = 20
```

DP:

```text
[0,10,20,0]
```

---

# i = 3

```text
left = 20 + abs(20-40)
      = 40
```

```text
right = 10 + abs(20-30)
       = 20
```

```text
dp[3] = min(40,20)
       = 20
```

Final DP:

```text
[0,10,20,20]
```

Answer:

```text
20
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(N) |
| Space Complexity | O(N) |

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

# 📌 Variables Meaning

```text
prev
=
dp[i-1]
```

```text
prev2
=
dp[i-2]
```

---

# ❓ Why prev = 0 ?

Initially:

```text
dp[0] = 0
```

So:

```text
prev = 0
```

---

# ❓ Why prev2 = 0 ?

At:

```text
i = 1
```

2-step jump is impossible.

So initially:

```text
prev2
```

is not used.

We initialize it safely with:

```text
0
```

---

# 💻 Code

```cpp
class Solution {
public:

    int frogJump(vector<int>& height) {

        int n = height.size();

        int prev = 0;
        int prev2 = 0;

        for(int i = 1; i < n; i++) {

            int left =
                prev
                + abs(height[i] - height[i - 1]);

            int right = INT_MAX;

            if(i > 1) {

                right =
                    prev2
                    + abs(height[i] - height[i - 2]);
            }

            int cur = min(left, right);

            prev2 = prev;
            prev = cur;
        }

        return prev;
    }
};
```

---

# 🌳 Dry Run

Input:

```text
height = [20,30,40,20]
```

---

# Initial

```text
prev = 0
prev2 = 0
```

---

# i = 1

```text
left = 0 + 10
      = 10
```

2-step jump impossible.

```text
cur = 10
```

Update:

```text
prev2 = 0
prev = 10
```

---

# i = 2

```text
left = 10 + 10
      = 20
```

```text
right = 0 + 20
       = 20
```

```text
cur = 20
```

Update:

```text
prev2 = 10
prev = 20
```

---

# i = 3

```text
left = 20 + 20
      = 40
```

```text
right = 10 + 10
       = 20
```

```text
cur = 20
```

Update:

```text
prev2 = 20
prev = 20
```

Return:

```text
20
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(N) |
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

```text
“How did you derive the recurrence?”
```

Say:

```text
To reach stair i,
the last jump could either come from i-1
or from i-2.

Therefore,

f(i)
=
min(
    f(i-1) + jump cost,

    f(i-2) + jump cost
)
```

---

# ⚠️ Important Interview Points

## ❓ Why DP?

Because recursion repeats states.

Example:

```text
solve(1)
solve(0)
```

get calculated multiple times.

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
| Recursion | O(2^N) | O(N) |
| Memoization | O(N) | O(N) + O(N) |
| Tabulation | O(N) | O(N) |
| Space Optimization | O(N) | O(1) |

---

# ⚠️ Edge Cases

- only 1 stair
- all heights equal
- large jumps cheaper

---

# ❌ Common Mistakes

- Forgetting base case
- Forgetting:
  
```cpp
if(i > 1)
```

- Wrong recurrence relation
- Updating variables in wrong order
- Confusing:
  
```text
prev
prev2
```

---

# 🧩 Golden Memory Trick

```text
Think about LAST JUMP

Current answer
=
best among all valid previous states
```

---

# 🔥 DP Evolution

```text
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
```

---

# 🚨 SUPER IMPORTANT DP OBSERVATION

Whenever recurrence uses ONLY:

```text
dp[i-1]
dp[i-2]
```

like:

```text
f(i)
depends only on previous 2 states
```

then:

# ✅ SPACE OPTIMIZATION TO O(1) IS POSSIBLE

because we only need:

```text
previous 2 answers
```

NOT the full DP array.

---

# 🧠 Instant Recognition Trick

Whenever you see:

```text
minimum cost
+
1-step or 2-step jumps
```

Immediately think:

# Fibonacci Style DP

because:

```text
current answer depends on previous states
```
