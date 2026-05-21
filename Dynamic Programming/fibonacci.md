PROBLEM: Fibonacci Sequence

PATTERN:
Dynamic Programming

WHY THIS PATTERN:
Same recursive calls repeat.
Problem has overlapping subproblems.
Current answer depends on previous answers.

CORE IDEA:
Fibonacci follows:

F(n) = F(n-1) + F(n-2)

We optimize repeated recursive calls using DP.

STEP 1 — RECURSION (Brute Force)
Intuition
Directly follow recurrence relation.
To calculate:

fib(n)

we need:

fib(n-1)
fib(n-2)


Recursive Code

class Solution {
public:

    int fib(int n) {

        // base case
        if(n <= 1)
            return n;

        return fib(n-1) + fib(n-2);
    }
};


Recursive Dry Run
Find:

fib(5)

Calls:

fib(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   └── fib(1)
│   └── fib(2)
└── fib(3)

Notice:

fib(3) repeated
fib(2) repeated

This is the problem.

TC and SC
Time Complexity

O(2^n)

Because each call branches into two more calls.

Space Complexity

O(n)

Due to recursion stack.

WHY RECURSION IS BAD HERE
Too many repeated computations.
Example:

fib(40)

becomes extremely slow.

STEP 2 — MEMOIZATION (Top Down DP)
WHY MEMOIZATION?
We already solved some subproblems.
Why solve again?
Store answers in DP array.

KEY IDEA

dp[n] = fib(n)

Before computing:
check if answer already exists.

IMPORTANT INTERVIEW POINT
Always say:

"We are caching overlapping subproblems."

Interviewers love this line.

Memoization Code

class Solution {
public:

    int solve(int n, vector<int>& dp) {

        // base case
        if(n <= 1)
            return n;

        // already computed
        if(dp[n] != -1)
            return dp[n];

        // compute and store
        dp[n] = solve(n-1, dp) + solve(n-2, dp);

        return dp[n];
    }

    int fib(int n) {

        vector<int> dp(n+1, -1);

        return solve(n, dp);
    }
};


IMPORTANT DOUBT
“Why not store dp[0] and dp[1]?”
We CAN.
Example:

dp[0] = 0;
dp[1] = 1;

But many people directly return base case:

if(n <= 1)
    return n;

Both are correct.

Memoization Dry Run
Initially:

dp = [-1,-1,-1,-1,-1,-1]


Find fib(5)
Need:

fib(4) + fib(3)


fib(2)

fib(1) + fib(0)
= 1 + 0
= 1

Store:

dp[2] = 1


fib(3)

fib(2) + fib(1)
= 1 + 1
= 2

Store:

dp[3] = 2


fib(4)

fib(3) + fib(2)
= 2 + 1
= 3

Store:

dp[4] = 3


fib(5)

fib(4) + fib(3)
= 3 + 2
= 5

Store:

dp[5] = 5


Final DP Array

[-1,-1,1,2,3,5]


TC and SC
Time Complexity

O(n)

Each state computed once.

Space Complexity
DP Array:

O(n)

Recursion Stack:

O(n)

Total:

O(n)


STEP 3 — TABULATION (Bottom Up DP)
WHY TABULATION?
Memoization still uses recursion stack.
Can we build answers iteratively?
YES.

CORE IDEA
Instead of:

n → n-1 → n-2

go:

0 → 1 → 2 → 3 → n

Build answers from smaller states.

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


Important Observation

dp[i]

means:

fibonacci of i

Always define DP state clearly.

Tabulation Dry Run
Initially:

dp = [0,1,_,_,_,_]


i = 2

dp[2] = 1


dp = [0,1,1,_,_,_]


i = 3

dp[3] = 2


dp = [0,1,1,2,_,_]


i = 4

dp[4] = 3


dp = [0,1,1,2,3,_]


i = 5

dp[5] = 5

Final:

dp = [0,1,1,2,3,5]


TC and SC
Time Complexity

O(n)


Space Complexity

O(n)


STEP 4 — SPACE OPTIMIZATION
IMPORTANT OBSERVATION
To calculate current Fibonacci, we only need:

previous two values

NOT entire DP array.

WHY THIS WORKS
Formula only depends on:

F(n) = F(n-1) + F(n-2)

So storing all states is unnecessary.

CORE IDEA
Instead of:

dp[i-1]
dp[i-2]

store only:

prev1
prev2


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


VERY IMPORTANT DOUBT
“Why no curr[i] ?”
Because:

curr

is NOT an array.
It is just one variable storing current answer.

Meaning of Variables


Space Optimization Dry Run
Initially:

prev2 = 0
prev1 = 1


i = 2

curr = 1

Update:

prev2 = 1
prev1 = 1


i = 3

curr = 2

Update:

prev2 = 1
prev1 = 2


i = 4

curr = 3

Update:

prev2 = 2
prev1 = 3


i = 5

curr = 5

Update:

prev2 = 3
prev1 = 5

Answer:

5


FINAL COMPLEXITIES












HOW TO EXPLAIN IN INTERVIEW
Step 1
Say:

"This problem has overlapping subproblems."


Step 2
Write recursion first.
Shows:
recurrence understanding
problem clarity

Step 3
Point out repeated calls.
Example:

fib(3)
fib(2)

repeat many times.

Step 4
Say:

"We can cache computed states using DP."

Move to memoization.

Step 5
Then say:

"Since recursion stack still exists, we can convert this into iterative tabulation."


Step 6
Finally say:

"We only need previous two states, so we can optimize space."

Interviewers LOVE this progression.

EDGE CASES

n = 0
n = 1
small inputs
large n causing recursion TLE


WHY I GOT STUCK / MIGHT FORGET
Forgetting DP state meaning
Forgetting base cases
Mixing memoization and tabulation
Forgetting why space optimization works
Confusing curr with curr[i]
Forgetting recursion stack space in memoization

GOLDEN MEMORY TRICK

Recursion repeats
→ Store answers
→ Build iteratively
→ Remove extra storage

This is Dynamic Programming in one sentence.
