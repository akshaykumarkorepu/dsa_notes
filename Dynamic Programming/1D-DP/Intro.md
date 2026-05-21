# 🚀 Dynamic Programming (Concept + Shortcut Notes + 1D DP)

---

# 📌 What is Dynamic Programming?

Dynamic Programming (DP) is an optimization technique used when:

- Problems can be broken into smaller subproblems
- Same subproblems repeat again and again
- We store answers to avoid recomputation

---

# 🧠 Core Philosophy of DP

```text
Solve once → Store answer → Reuse later
```

---

# ✅ Two Conditions for DP

---

## 1. Overlapping Subproblems

Same problems repeat multiple times.

### Example

```text
fib(5)
```

computes:

```text
fib(3)
```

multiple times.

---

## 2. Optimal Substructure

Big answer depends on smaller answers.

### Example

```text
F(n) = F(n-1) + F(n-2)
```

---

# 🌟 Golden DP Flow

Almost every DP problem follows this order:

```text
1. Recursion
2. Memoization
3. Tabulation
4. Space Optimization
```

---

# 📚 DP Terminologies

| Term | Meaning |
|---|---|
| State | What DP array stores |
| Transition | Relation between states |
| Base Case | Smallest valid answer |
| Memoization | Top-down DP |
| Tabulation | Bottom-up DP |

---

# ⭐ Most Important Thing in DP

Always define:

```text
dp[i] means what?
```

### Example

```text
dp[i] = fibonacci of i
```

If state definition is clear,
DP becomes easy.

---

# ⚔️ Memoization vs Tabulation

| Memoization | Tabulation |
|---|---|
| Recursive | Iterative |
| Top-down | Bottom-up |
| Uses recursion stack | No recursion stack |
| Easier to think | More optimized |

---

# 🔍 How To Identify DP Questions

Usually when problem involves:

- minimum
- maximum
- count ways
- longest
- shortest
- recursion repeats
- choices at every step

---

# 🧩 One-Line DP Memory Trick

```text
Recursion + Storage = Dynamic Programming
```

---

# 🚀 DYNAMIC PROGRAMMING SHORTCUT NOTES

---

# 🧠 CORE DP THINKING

Dynamic Programming is mainly about:

```text
1. Define the STATE
2. Try all possible CHOICES
3. Combine the answers
4. Store the result
```

---

# 🔹 STEP 1 → REPRESENT THE PROBLEM IN TERMS OF STATE

Ask yourself:

```text
“What uniquely identifies a subproblem?”
```

This becomes the DP state.

Usually:

```text
f(i)
f(i, j)
f(index, capacity)
f(row, col)
f(i, prev)
```

---

# 📌 EXAMPLES OF STATES

---

## Fibonacci

```text
f(n)
```

Meaning:

```text
Answer for nth Fibonacci number
```

---

## Climbing Stairs

```text
f(i)
```

Meaning:

```text
Number of ways to reach stair i
```

---

## Grid Problems

```text
f(row, col)
```

Meaning:

```text
Answer starting from cell (row, col)
```

---

## Knapsack

```text
f(index, capacity)
```

Meaning:

```text
Best answer using remaining items and remaining capacity
```

---

# 🔹 STEP 2 → DO ALL POSSIBLE STUFF ON THAT STATE

This is the MOST IMPORTANT DP STEP.

At every state:

```text
Try all possible choices
```

OR

```text
Try all possible moves / transitions
```

---

# 📌 EXAMPLES

---

## Climbing Stairs

From stair i:

```text
1 step
2 steps
```

Recurrence:

```text
f(i) = f(i+1) + f(i+2)
```

---

## Knapsack

At item i:

```text
Take
Not Take
```

Recurrence:

```text
f(i, W) = max(take, not_take)
```

---

## Grid Paths

From (r,c):

```text
Go Right
Go Down
```

---

## LIS

At index i:

```text
Include current element
Skip current element
```

---

# 🔹 STEP 3 → HOW TO COMBINE ANSWERS

This depends on the question.

---

# ✅ CASE 1 → COUNT ALL WAYS → SUM

If question says:

- Count
- Number of ways
- Total ways
- Total combinations
- Total paths

Then:

```text
Answer = sum(all choices)
```

---

# 📌 EXAMPLES

---

## Unique Paths

```text
ways = right + down
```

---

## Coin Change II

```text
totalWays = take + notTake
```

---

## Decode Ways

```text
totalWays = sum of all valid recursive calls
```

---

# 🔑 KEY IDEA

```text
Every valid way contributes
```

So we ADD them.

---

# ✅ CASE 2 → MIN / MAX PROBLEMS

If question says:

- Minimum
- Maximum
- Longest
- Shortest
- Best
- Optimal

Then:

```text
Answer = min(...)
```

OR

```text
Answer = max(...)
```

---

# 📌 MIN EXAMPLES

---

## Minimum Coins

```text
ans = min(take, notTake)
```

---

## Minimum Path Sum

```text
ans = grid[r][c] + min(right, down)
```

---

# 📌 MAX EXAMPLES

---

## LIS

```text
ans = max(include, exclude)
```

---

## Maximum Profit

```text
ans = max(buy, sell, skip)
```

---

# 🔑 KEY IDEA

```text
Among all choices,
choose the BEST one
```

---

# 🌟 UNIVERSAL DP TEMPLATE

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

# 🎯 GOLDEN INTERVIEW THINKING

Whenever stuck in DP:

Ask these 3 questions.

---

# ❓ QUESTION 1

```text
What is my state?
```

---

# ❓ QUESTION 2

```text
What choices can I make from this state?
```

---

# ❓ QUESTION 3

```text
Am I:
- counting all ways?
OR
- finding the best way?
```

---

# ⚡ REAL DP FORMULA

DP is basically:

```text
State + Choices + Transition
```

OR

```text
Current Position
→ Try all moves
→ Store answer
```

---

# 🧩 ONE-LINE DP DEFINITION

```text
DP = Recursion + Memoization
```

---

# 🚀 FINAL DP SHORTCUT

```text
Represent problem as STATE

At each state:
→ Try all possible choices

If counting:
→ SUM all choices

If optimization:
→ MIN/MAX all choices

Store answer
```
