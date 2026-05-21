Problem: Fibonacci Sequence
Pattern

Dynamic Programming

Why This Pattern?
Overlapping subproblems
Same recursive calls repeat
Current state depends on previous states
Core Recurrence

F(n)=F(n−1)+F(n−2)

Approach 1 — Recursion (Brute Force)
Intuition

To calculate:

fib(n)

we need:

fib(n-1)
fib(n-2)

Directly follow recurrence relation.

Code
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

Repeated computations cause inefficiency.

Complexity Analysis
Time Complexity
O(2^n)
Space Complexity
O(n)

Reason:
Recursion stack space.

Why Recursion is Bad?

For larger inputs like:

fib(40)

the number of recursive calls becomes extremely large.

Approach 2 — Memoization (Top Down DP)
Key Idea

Store already computed states.

dp[n] = fib(n)

Before computing:

check if answer already exists
Important Interview Point
"We are caching overlapping subproblems."
Code
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
Initial DP Array
[-1,-1,-1,-1,-1,-1]
Final DP Array
[-1,-1,1,2,3,5]
Complexity Analysis
Time Complexity
O(n)
Space Complexity
O(n)

Reason:

DP array
recursion stack
Approach 3 — Tabulation (Bottom Up DP)
Key Idea

Instead of solving:

n → n-1 → n-2

build answers from:

0 → 1 → 2 → 3 → n
DP State
dp[i] = fibonacci of i
Code
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
Dry Run
Initial
[0,1,_,_,_,_]
Final
[0,1,1,2,3,5]
Complexity Analysis
Time Complexity
O(n)
Space Complexity
O(n)
Approach 4 — Space Optimization
Important Observation

We only need:

previous value
second previous value

Entire DP array is unnecessary.

Key Idea

Instead of storing:

dp[i-1]
dp[i-2]

store only:

prev1
prev2
Code
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
Complexity Analysis
Time Complexity
O(n)
Space Complexity
O(1)
Interview Flow
Step 1

Explain recursion.

Step 2

Show repeated calls.

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
Common Mistakes
Forgetting base cases
Mixing memoization and tabulation
Forgetting DP state meaning
Confusing curr with curr[i]
Golden Memory Trick
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
