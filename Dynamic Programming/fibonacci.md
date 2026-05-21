Fibonacci Sequence
Pattern

Dynamic Programming

Why This Pattern?
Overlapping subproblems
Same recursive calls repeat
Current state depends on previous states
Core Recurrence

F(n)=F(n−1)+F(n−2)

Step 1 — Recursion (Brute Force)
Intuition

To calculate:

fib(n)

we need:

fib(n-1)
fib(n-2)

Directly follow the recurrence relation.

Recursive Code
class Solution {
public:

    int fib(int n) {

        if(n <= 1)
            return n;

        return fib(n-1) + fib(n-2);
    }
};
Dry Run
fib(5)

├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   └── fib(1)
│   └── fib(2)
└── fib(3)
Observation
fib(3) repeats
fib(2) repeats

This causes repeated computation.

Complexity
Time Complexity
O(2^n)
Space Complexity
O(n)

Reason:
Recursion stack.

Why Recursion is Bad?

For large inputs like:

fib(40)

the recursion becomes extremely slow due to repeated calls.

Step 2 — Memoization (Top Down DP)
Key Idea

Store already computed answers.

dp[n] = fib(n)

Before solving:

check if answer already exists
Important Interview Line
"We are caching overlapping subproblems."
Memoization Code
class Solution {
public:

    int solve(int n, vector<int>& dp) {

        if(n <= 1)
            return n;

        if(dp[n] != -1)
            return dp[n];

        dp[n] = solve(n-1, dp) + solve(n-2, dp);

        return dp[n];
    }

    int fib(int n) {

        vector<int> dp(n+1, -1);

        return solve(n, dp);
    }
};
Dry Run

Initial DP:

[-1,-1,-1,-1,-1,-1]

After computation:

[-1,-1,1,2,3,5]
Complexity
Time Complexity
O(n)
Space Complexity
O(n)
DP array
recursion stack
Step 3 — Tabulation (Bottom Up DP)
Core Idea

Instead of solving:

n → n-1 → n-2

build answers from:

0 → 1 → 2 → 3 → n
Tabulation Code
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
DP State Meaning
dp[i] = fibonacci of i
Complexity
Time Complexity
O(n)
Space Complexity
O(n)
Step 4 — Space Optimization
Important Observation

We only need:

previous value
second previous value

Entire DP array is unnecessary.

Space Optimized Code
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
Complexity
Time Complexity
O(n)
Space Complexity
O(1)
Interview Flow
Step 1

Explain recursion.

Step 2

Point out repeated calls.

Step 3

Introduce memoization.

Step 4

Convert to tabulation.

Step 5

Optimize space.

Edge Cases
n = 0
n = 1
very large n
Golden Memory Trick
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
