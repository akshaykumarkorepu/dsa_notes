🚀 Fibonacci Sequence
📌 Pattern

Dynamic Programming

🧠 Why This Pattern?
Overlapping subproblems
Repeated recursive calls
Current state depends on previous states
🔁 Recurrence Relation

F(n)=F(n−1)+F(n−2)

1️⃣ Recursion (Brute Force)
✅ Intuition

To calculate:

fib(n)

we need:

fib(n-1)
fib(n-2)

Directly follow recurrence relation.

💻 Code
class Solution {
public:

    int fib(int n) {

        if(n <= 1)
            return n;

        return fib(n-1) + fib(n-2);
    }
};
🌳 Dry Run
fib(5)

├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   └── fib(1)
│   └── fib(2)
└── fib(3)
⚠️ Observation

Repeated calls:

fib(3)
fib(2)

This causes repeated computation.

⏱️ Complexity Analysis
Complexity	Value
Time Complexity	O(2^n)
Space Complexity	O(n)
2️⃣ Memoization (Top Down DP)
🔑 Key Idea

Store already computed answers.

dp[n] = fib(n)

Before computing:

check if answer already exists
🎯 Important Interview Point

"We are caching overlapping subproblems."

💻 Code
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
🌳 Dry Run
Initial DP
[-1,-1,-1,-1,-1,-1]
Final DP
[-1,-1,1,2,3,5]
⏱️ Complexity Analysis
Complexity	Value
Time Complexity	O(n)
Space Complexity	O(n)
3️⃣ Tabulation (Bottom Up DP)
🔑 Key Idea

Instead of:

n → n-1 → n-2

build answers from:

0 → 1 → 2 → 3 → n
📌 DP State
dp[i] = fibonacci of i
💻 Code
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
⏱️ Complexity Analysis
Complexity	Value
Time Complexity	O(n)
Space Complexity	O(n)
4️⃣ Space Optimization
🔑 Important Observation

We only need:

previous value
second previous value

Entire DP array is unnecessary.

💻 Code
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
⏱️ Complexity Analysis
Complexity	Value
Time Complexity	O(n)
Space Complexity	O(1)
🎤 Interview Flow
Explain recursion
Show repeated calls
Introduce memoization
Convert to tabulation
Optimize space
⚠️ Edge Cases
n = 0
n = 1
very large n
❌ Common Mistakes
Forgetting base cases
Mixing memoization and tabulation
Forgetting DP state meaning
Confusing curr with curr[i]
🧩 Golden Memory Trick
Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage
