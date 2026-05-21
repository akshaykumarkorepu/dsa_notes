# 🚀 Dynamic Programming (Concept and 1D DP)

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
