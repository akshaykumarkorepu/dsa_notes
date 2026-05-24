```text
# NOTE

PROBLEM: Number of Paths / Unique Paths

PATTERN: 2D Grid DP

WHY THIS PATTERN:
At every cell:
- multiple choices exist
- answer depends on smaller subproblems

This is classic:
Grid + Count Ways + Move Right/Down

which immediately signals:
2D DP

--------------------------------------------------

CORE IDEA

To reach any cell (i,j):

You can only come from:
- TOP  -> (i-1,j)
- LEFT -> (i,j-1)

So recurrence becomes:

f(i,j) = f(i-1,j) + f(i,j-1)

Meaning:
ways to current cell
=
ways from top
+
ways from left

--------------------------------------------------

STATE DEFINITION

f(i,j)
=
number of ways to reach cell (i,j)

OR in DP:

dp[i][j]
=
number of ways to reach cell (i,j)

--------------------------------------------------

EDGE CASES

- m = 1
- n = 1
- first row
- first column
- out of bounds

--------------------------------------------------

WHY I MIGHT FORGET

- Why answer becomes 6
- Why we use top + left
- Why recursion goes negative
- How memoization converts to tabulation
- Why curr uses current row but prev uses previous row

--------------------------------------------------

INTERVIEW FLOW

If this question comes:

Immediately think:

Grid DP
Count total ways
Move only right/down

Then say:

For any cell,
I can come only from top or left.

So recurrence becomes:

f(i,j)=f(i-1,j)+f(i,j-1)

Then explain:
1. Recursion
2. Overlapping subproblems
3. Memoization
4. Tabulation
5. Space optimization

That is the PERFECT DP interview flow.

==================================================
==================== RECURSION ===================
==================================================

RECURSION IDEA

For current cell:
- try going UP
- try going LEFT

Add both answers.

--------------------------------------------------

RECURSION RECURRENCE

f(i,j)=f(i-1,j)+f(i,j-1)

--------------------------------------------------

BASE CASES

1. Reached Start

if(i == 0 && j == 0)
    return 1;

2. Outside Grid

if(i < 0 || j < 0)
    return 0;

Invalid path contributes:
0 ways

--------------------------------------------------

RECURSION CODE

class Solution {
public:

    int solve(int i, int j)
    {
        // Reached start
        if(i == 0 && j == 0)
            return 1;

        // Outside grid
        if(i < 0 || j < 0)
            return 0;

        int up = solve(i - 1, j);

        int left = solve(i, j - 1);

        return up + left;
    }

    int numberOfPaths(int m, int n)
    {
        return solve(m - 1, n - 1);
    }
};

--------------------------------------------------

RECURSION DRY RUN

Example:

m = 3
n = 3

Need:

solve(2,2)

FLOW:

solve(2,2)
=
solve(1,2)
+
solve(2,1)

Then:

solve(1,2)
=
solve(0,2)
+
solve(1,1)

Then:

solve(0,2)
=
solve(-1,2)
+
solve(0,1)

solve(-1,2)=0

because outside grid.

Eventually:

solve(1,1)

gets recalculated many times.

That is:
Overlapping Subproblem

--------------------------------------------------

RECURSION TC

O(2^(m+n))

--------------------------------------------------

RECURSION SC

O(m+n)

Reason:
recursion stack depth

==================================================
=================== MEMOIZATION ==================
==================================================

MEMOIZATION IDEA

Store already computed states.

If state already solved:
- directly return answer

Avoid repeated recursion.

--------------------------------------------------

DP STATE

dp[i][j]
=
number of ways to reach (i,j)

--------------------------------------------------

MEMOIZATION CODE

class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& dp)
    {
        // Reached start
        if(i == 0 && j == 0)
            return 1;

        // Outside grid
        if(i < 0 || j < 0)
            return 0;

        // Already solved
        if(dp[i][j] != -1)
            return dp[i][j];

        int up = solve(i - 1, j, dp);

        int left = solve(i, j - 1, dp);

        return dp[i][j] = up + left;
    }

    int numberOfPaths(int m, int n)
    {
        vector<vector<int>> dp(m, vector<int>(n, -1));

        return solve(m - 1, n - 1, dp);
    }
};

--------------------------------------------------

IMPORTANT MEMOIZATION SNIPPET

if(dp[i][j] != -1)
    return dp[i][j];

This line:
kills repeated recursion

--------------------------------------------------

MEMOIZATION DRY RUN

Suppose:

solve(1,1)

computed once:

2

Store:

dp[1][1] = 2

Next time recursion asks:

solve(1,1)

directly return:
2

No subtree recursion again.

--------------------------------------------------

MEMOIZATION TC

O(m*n)

Because:
each state computed once

--------------------------------------------------

MEMOIZATION SC

O(m*n) + O(m+n)

Reason:
- DP table
- recursion stack

==================================================
=================== TABULATION ===================
==================================================

TABULATION IDEA

Memoization:
Top Down

Tabulation:
Bottom Up

Instead of recursion:
fill DP table manually

--------------------------------------------------

TABULATION RECURRENCE

dp[i][j]=dp[i-1][j]+dp[i][j-1]

--------------------------------------------------

TABULATION THINKING

Current cell needs:
- top
- left

So loop order must ensure:
- top already filled
- left already filled

Thus:

Top -> Bottom
Left -> Right

--------------------------------------------------

TABULATION CODE

class Solution {
public:

    int numberOfPaths(int m, int n)
    {
        vector<vector<int>> dp(m, vector<int>(n, 0));

        dp[0][0] = 1;

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                // Base cell already filled
                if(i == 0 && j == 0)
                    continue;

                int up = 0;
                int left = 0;

                if(i > 0)
                    up = dp[i - 1][j];

                if(j > 0)
                    left = dp[i][j - 1];

                dp[i][j] = up + left;
            }
        }

        return dp[m - 1][n - 1];
    }
};

--------------------------------------------------

IMPORTANT TABULATION SNIPPETS

Base Case:

dp[0][0] = 1;

Transition:

dp[i][j] = up + left;

--------------------------------------------------

TABULATION DRY RUN

Final DP table for:

m = 3
n = 3

becomes:

1 1 1
1 2 3
1 3 6

--------------------------------------------------

HOW 6 COMES

At:

dp[2][2]

Top contributes:

dp[1][2] = 3

Left contributes:

dp[2][1] = 3

So:

dp[2][2] = 3 + 3 = 6

--------------------------------------------------

TABULATION TC

O(m*n)

--------------------------------------------------

TABULATION SC

O(m*n)

Reason:
full DP matrix

==================================================
=============== SPACE OPTIMIZATION ===============
==================================================

SPACE OPTIMIZATION IDEA

Current row only needs:
- previous row
- current row left value

Entire matrix unnecessary.

--------------------------------------------------

OBSERVATION

dp[i][j]=dp[i-1][j]+dp[i][j-1]

Needs only:
- previous row
- current row previous column

So compress matrix.

--------------------------------------------------

VARIABLES

prev[j]

Represents:

top value
dp[i-1][j]

curr[j]

Represents:

current row value
dp[i][j]

--------------------------------------------------

FLOW

While building current row:
- left already exists inside curr
- top exists in prev

After row finishes:

prev = curr;

Current row becomes previous row.

--------------------------------------------------

SPACE OPTIMIZED CODE

class Solution {
public:

    int numberOfPaths(int m, int n)
    {
        vector<int> prev(n, 0);

        for(int i = 0; i < m; i++)
        {
            vector<int> curr(n, 0);

            for(int j = 0; j < n; j++)
            {
                if(i == 0 && j == 0)
                {
                    curr[j] = 1;
                    continue;
                }

                int up = 0;
                int left = 0;

                if(i > 0)
                    up = prev[j];

                if(j > 0)
                    left = curr[j - 1];

                curr[j] = up + left;
            }

            prev = curr;
        }

        return prev[n - 1];
    }
};

--------------------------------------------------

IMPORTANT SPACE OPT SNIPPETS

TOP

up = prev[j];

LEFT

left = curr[j - 1];

SHIFT ROW

prev = curr;

--------------------------------------------------

SPACE OPT DRY RUN

After Row 0

prev = [1 1 1]

--------------------------------------------------

Build Row 1

curr = [1 2 3]

Then:

prev = [1 2 3]

--------------------------------------------------

Build Row 2

curr = [1 3 6]

Final:

prev[2] = 6

--------------------------------------------------

SPACE OPT TC

O(m*n)

--------------------------------------------------

SPACE OPT SC

O(n)

==================================================
============ MOST IMPORTANT INTERVIEW LINES ======
==================================================

If interviewer asks recurrence

Say:

To reach any cell,
I can only come from top or left.

So:

f(i,j)=f(i-1,j)+f(i,j-1)

--------------------------------------------------

If interviewer asks memoization intuition

Say:

Recursive solution recalculates same states repeatedly.

So I store answers in DP table
to avoid repeated computation.

--------------------------------------------------

If interviewer asks tabulation intuition

Say:

Instead of recursion deciding order automatically,
I fill DP table manually
such that required previous states
are already computed.

--------------------------------------------------

If interviewer asks space optimization intuition

Say:

Current row only depends on previous row.

So full matrix unnecessary.

I can reduce space from O(m*n) to O(n).

==================================================
================ FINAL DP TAKEAWAY ===============
==================================================

For Grid DP always ask:

1. What does state mean?
2. What recurrence builds it?
3. Which previous states are needed?
4. Can I reduce dimensions?

That is the complete DP thinking process.
```
