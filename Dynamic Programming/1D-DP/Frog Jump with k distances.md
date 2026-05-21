# 🐸 Frog Jump with K Distances

> Classic Dynamic Programming problem where frog can jump up to `K` steps.

---

# 📌 Pattern

| Category | Type |
|----------|------|
| Pattern | Dynamic Programming |
| Difficulty | Medium |
| Concepts | Recursion, Memoization, Tabulation |

---

# 🧠 Why This Pattern?

At every stair:
- Frog has multiple choices
- Same subproblems repeat
- We need minimum cost

Current answer depends on:
- previous states

This is:

# Dynamic Programming

---

# ❓ Problem Understanding

We are given:

```cpp
height[]
```

where:

```cpp
height[i]
```

represents height of the `i-th` stair.

---

## 📌 Frog Starts From

```text
index 0
```

---

## 📌 Goal

```text
reach index n-1
```

---

## 📌 Allowed Jumps

```text
1 step
2 steps
3 steps
...
K steps
```

---

## 📌 Jump Cost

```cpp
abs(height[i] - height[j])
```

---

# 🎯 Need

```text
MINIMUM TOTAL ENERGY
```

---

# ⚡ DP Shortcut Thinking

---

## 📌 STEP 1 → DEFINE THE STATE

```text
f(i)

= minimum energy needed to reach stair i
```

---

## 📌 STEP 2 → FIND CHOICES

To reach stair `i`

frog can come from:

```text
i-1
i-2
i-3
...
i-k
```

---

## 📌 STEP 3 → FORM RECURRENCE

### Core Recurrence

```text
f(i) = minimum of all possible jumps
```

---

### Complete Formula

```text
f(i) =
min(
    f(i-j) + abs(height[i] - height[i-j])
)

where:

1 <= j <= k
```

---

## 📌 Mathematical Form

```text
                k
f(i) = min   [ f(i-j) + |height[i]-height[i-j]| ]
         1<=j<=k
```

---

## 📌 STEP 4 → BASE CASE

```text
f(0) = 0
```

Because:

```text
Already standing at stair 0
```

---

# 💡 Core Idea

```text
At every stair:

Try all jumps from 1 to K
Take minimum energy
```

---

# ⚠️ Edge Cases

```text
Only 1 stair
K greater than N
All heights same
Bigger jump may be cheaper
```

---

# ❌ Common Mistakes

```text
Forgetting loop from 1 → K

Forgetting boundary check:

if(ind-j >= 0)

Forgetting this is:
MINIMUM PATH DP
```

---

<br>

# 1️⃣ Recursion (Brute Force)

---

# 💡 Intuition

To reach stair `ind`

Try all jumps:

```text
ind-1
ind-2
...
ind-k
```

Take minimum.

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int ind, vector<int>& height, int k) {

        // Base Case
        if(ind == 0)
            return 0;

        int mini = INT_MAX;

        // Try all jumps
        for(int j = 1; j <= k; j++) {

            if(ind - j >= 0) {

                int jump =
                    solve(ind - j, height, k)
                    + abs(height[ind] - height[ind - j]);

                mini = min(mini, jump);
            }
        }

        return mini;
    }

    int frogJump(vector<int>& height, int k) {

        int n = height.size();

        return solve(n - 1, height, k);
    }
};
```

---

# 🔥 Important Recursion Snippets

## 📌 Base Case

```cpp
if(ind == 0)
    return 0;
```

---

## 📌 Try All Jumps

```cpp
for(int j = 1; j <= k; j++)
```

---

## 📌 Boundary Check

```cpp
if(ind - j >= 0)
```

---

## 📌 Transition

```cpp
solve(ind-j)
+
abs(height[ind] - height[ind-j])
```

---

## 📌 Most Important Line

```cpp
mini = min(mini, jump);
```

Because:

```text
We need MINIMUM energy
```

---

# 🌳 Dry Run

Input:

```text
height = [10,20,30,10]
k = 3
```

Need:

```text
solve(3)
```

---

## 📌 Jump 1 Step

```text
solve(2)
+
abs(10 - 30)

= solve(2) + 20
```

---

## 📌 Jump 2 Steps

```text
solve(1)
+
abs(10 - 20)

= solve(1) + 10
```

---

## 📌 Jump 3 Steps

```text
solve(0)
+
abs(10 - 10)

= 0
```

---

## ✅ Minimum Energy

```text
0
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(K^N) |
| Space Complexity | O(N) |

---

<br>

# 2️⃣ Memoization (Top Down DP)

---

# 💡 Key Idea

Store already computed answers.

---

## 📌 DP State

```text
dp[i]

= minimum energy needed to reach stair i
```

---

# 💻 Code

```cpp
class Solution {
public:

    int solve(int ind,
              vector<int>& height,
              vector<int>& dp,
              int k) {

        if(ind == 0)
            return 0;

        // Already Computed
        if(dp[ind] != -1)
            return dp[ind];

        int mini = INT_MAX;

        for(int j = 1; j <= k; j++) {

            if(ind - j >= 0) {

                int jump =
                    solve(ind - j, height, dp, k)
                    + abs(height[ind] - height[ind - j]);

                mini = min(mini, jump);
            }
        }

        return dp[ind] = mini;
    }

    int frogJump(vector<int>& height, int k) {

        int n = height.size();

        vector<int> dp(n, -1);

        return solve(n - 1, height, dp, k);
    }
};
```

---

# 🔥 Important Memoization Snippets

## 📌 DP Check

```cpp
if(dp[ind] != -1)
    return dp[ind];
```

---

## 📌 Store Answer

```cpp
return dp[ind] = mini;
```

---

# 🌳 Dry Run

Initial DP:

```text
[-1,-1,-1,-1]
```

---

After Solving:

```text
dp[1] = 10
dp[2] = 20
dp[3] = 0
```

---

Final DP:

```text
[-1,10,20,0]
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(N*K) |
| Space Complexity | O(N)+O(N) |

---

# 📌 Why O(N*K)?

```text
For every index:

we try K transitions
```

---

<br>

# 3️⃣ Tabulation (Bottom Up DP)

---

# 💡 Key Idea

Instead of recursion:

```text
small → large
```

Build answers iteratively.

---

# 📌 DP State

```text
dp[i]

= minimum energy needed to reach stair i
```

---

# 💻 Code

```cpp
class Solution {
public:

    int frogJump(vector<int>& height, int k) {

        int n = height.size();

        vector<int> dp(n, 0);

        dp[0] = 0;

        for(int i = 1; i < n; i++) {

            int mini = INT_MAX;

            for(int j = 1; j <= k; j++) {

                if(i - j >= 0) {

                    int jump =
                        dp[i - j]
                        + abs(height[i] - height[i - j]);

                    mini = min(mini, jump);
                }
            }

            dp[i] = mini;
        }

        return dp[n - 1];
    }
};
```

---

# 🔥 Important Tabulation Snippets

## 📌 Try All Previous K States

```cpp
for(int j = 1; j <= k; j++)
```

---

## 📌 Transition

```cpp
jump =
dp[i-j]
+
abs(height[i] - height[i-j])
```

---

## 📌 Store Answer

```cpp
dp[i] = mini;
```

---

## 📌 Final Answer

```cpp
return dp[n-1];
```

---

# 🌳 Dry Run

Input:

```text
height = [10,20,30,10]
k = 3
```

---

## 📌 Initial DP

```text
[0,0,0,0]
```

---

## 📌 i = 1

```text
0 + abs(20-10)
= 10
```

```text
dp[1] = 10
```

DP:

```text
[0,10,0,0]
```

---

## 📌 i = 2

```text
10 + 10 = 20
0 + 20 = 20
```

```text
dp[2] = 20
```

DP:

```text
[0,10,20,0]
```

---

## 📌 i = 3

```text
20 + 20 = 40
10 + 10 = 20
0 + 0 = 0
```

```text
dp[3] = 0
```

Final DP:

```text
[0,10,20,0]
```

---

# ✅ Final Answer

```text
dp[n-1] = 0
```

---

# ⏱️ Complexity Analysis

| Complexity | Value |
|-----------|-------|
| Time Complexity | O(N*K) |
| Space Complexity | O(N) |

---

<br>

# 4️⃣ Space Optimization?

---

# 💡 Important Observation

In normal Frog Jump:

```text
dp[i-1]
dp[i-2]
```

Only 2 states needed.

So:

```text
O(1) optimization possible
```

---

# ❌ Here Full O(1) Optimization Is NOT Possible

Because now we need:

```text
dp[i-1]
dp[i-2]
dp[i-3]
...
dp[i-k]
```

We may need:

```text
K previous states
```

So:

```text
Full O(1) optimization NOT possible
```

---

# ✅ Best Possible Space

```text
O(N)
```

(Or advanced O(K) optimization)

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
```

---

# 🧠 Interview Explanation

If interviewer asks:

> “How did you derive the recurrence?”

Say:

```text
To reach stair i,

frog can jump from:
i-1
i-2
...
i-k

So we try all possible jumps
and take minimum energy.
```

---

# ⚠️ Important Interview Points

## ❓ Why DP?

```text
Because recursion repeats states
```

---

## ❓ Why Memoization?

```text
To avoid recomputing subproblems
```

---

## ❓ Why Tabulation?

```text
Removes recursion stack
```

---

## ❓ Why No O(1) Space?

```text
Because we may need K previous states
```

---

# 📊 Complete Complexity Table

| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| Recursion | O(K^N) | O(N) |
| Memoization | O(N*K) | O(N)+O(N) |
| Tabulation | O(N*K) | O(N) |

---

# 🧩 Golden Memory Trick

```text
minimum cost
+
multiple jump choices

=> DP WITH LOOP OVER CHOICES
```

---

# 🔥 Very Important DP Observation

Whenever recurrence uses:

```text
Try all previous K states
```

like:

```text
dp[i-1]
dp[i-2]
...
dp[i-k]
```

then:

```text
Time Complexity usually becomes:

O(N*K)
```

Because:

```text
For every index:

we try K transitions
```
