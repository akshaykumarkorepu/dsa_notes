Fibonacci Sequence: A Dynamic Programming Evolution
This guide explores the transition from brute-force recursion to space-optimized Dynamic Programming (DP) using the Fibonacci sequence as the primary case study.
📌 Pattern Overview
Pattern: Dynamic Programming
Core Concept: Solving problems by breaking them down into simpler subproblems and storing their solutions.
🧠 Why this pattern?
Overlapping Subproblems: The same calculation is performed multiple times.
Repeated```text
fib(5)
/      )
The most intuitive approach, directly translating the mathematical recurrence into code.
💻 Implementation (C++)
C++
class Solution {
public:
    int fib(int n) {
        if(n <= 1) return n;
        return fib(n-1) + fib(n-2);```
> **Observation:** Notice `fib(3)` and `fib(2)` are computed multiple times. This leads to exponential growth
    }
};
🌳 Dry Run: fib(5)
The recursion tree demonstrates significant redundancy:
Plaintext
fib.

| Complexity | Value |
| :--- | :--- |
| **Time Complexity** | $O(2^n)$ |
| **Space Complexity**(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2) <-- Repeated
│   │ | $O(n)$ (Recursive Stack) |

---

## 2️⃣ Memoization (Top-Down DP)
**Key Idea:** "   └── fib(1)
│   └── fib(2)     <-- Repeated
└── fib(3)         <-- Repeated
⏱️ Complexity
. Memoization (Top-Down DP)
Caching results to avoid redundant computations. This is often the most natural transition from recursion in an interview.
💻 Implementation (C++)
    return dp[n] = solve(n-1, dp) + solve(n-2, dp);
}

int fib(int n) {
    vector<int> dp(n+1, -1);
    return solve(n, dp);
}

| Complexity | Value |
| :--- | :--- |
| **Time Complexity** | $O(n)$ |
| **Space Complexity** | $O(n)$ (Array only) |

---

## 4️⃣ Space Optimization (The "Golden" Version)
**Observation:** To};
⏱️ Complexity
Metric	Value	Reason
Time Complexity	O(n)	Each state is computed exactly once.
Space Complexity	O(n)	O(n) for DP array + O(n) for recursion stack.
3. Tabulation (Bottom-Up DP)
Eliminating recursion entirely by building the solution iteratively from the base cases upward.
💻 Implementation (C++)
if(n <= 1) return n;
vector dp(n+1);
    dp[0] = 0;
    dp[1] = 1;

    for(int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
};
states. |
4. Space Optimization
Since we only ever need the last two values to calculate the current one, we can reduce space from linear to constant.
💻 Implementation (C++)
C++
class Solution {
public:
    int fib(int n) { Memoization:** Mention "Caching overlapping subproblems."
3.  **Iterate to Tabulation:** Explain why removing the stack overhead is better.
4.  **Finish
        if(n <= 1) return n;

        int prev2 = 0; // F(n-2)
        int prev1 = 1; // F(n-1)

        for(int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
};
⏱️ Complexity
Metric	Value	Reason
Time Complexity	O(n)	Single loop iteration.
Space Complexity	O(1)	Only using three integer variables.
🎤 Interview Strategy & Summary
⚠️ Common Mistakes to Avoid
Base Cases: Forgetting n=0 or n=1 often leads to runtime errors.
DP State: Not being able to define what dp[i] represents.
Integer Overflow: For large n, Fibonacci numbers quickly exceed the capacity of a 32-bit int.
Golden Memory Trick:
Recursion repeats → Store answers (Memo) → Build iteratively (Tabulation) → Remove extra storage (Space Optimization).
