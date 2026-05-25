# Minimum Path Sum

---

# SHORT DP NOTES

```text
PROBLEM:
Minimum Path Sum

PATTERN:
Grid DP

WHY THIS PATTERN:
We move on a grid and each cell depends on previous cells.

MOVES:
Down
Right

REVERSE THINKING:
To reach (i,j),
we can come from:
1. Top  -> (i-1, j)
2. Left -> (i, j-1)

CORE IDEA:
Current Cell Value +
minimum(top path, left path)

STATE:
dp[i][j]
=
minimum path sum to reach cell (i,j)

RECURRENCE:
dp[i][j] =
grid[i][j] + min(up, left)

BASE CASE:
dp[0][0] = grid[0][0]

INVALID CASE:
return 1e9

WHY 1e9:
Because we use MIN().
Invalid path should never become minimum.

WHY I MIGHT FORGET:
I may incorrectly use 0 instead of 1e9.
0 works for counting paths,
NOT for minimum problems.

INTERVIEW FLOW:
1. Explain recursive thinking
2. Show overlapping subproblems
3. Convert to memoization
4. Remove recursion -> tabulation
5. Reduce space -> space optimization
```

---

# QUESTION EXPLANATION

We are given a grid.

We start at:

```text
(0,0)
```

Need to reach:

```text
(n-1,m-1)
```

Allowed moves:

```text
Right
Down
```

We must return:

```text
minimum possible sum path
```

---

# MAIN DP THINKING

Instead of moving forward:

Think backward.

To reach:

```text
(i,j)
```

we could only come from:

```text
(i-1,j)
(i,j-1)
```

So:

```text
f(i,j)=grid[i][j]+min(f(i-1,j),f(i,j-1))
```

---

# RECURSION

# RECURSION IDEA

From every cell:

- move UP
- move LEFT

until reaching `(0,0)`.

Take minimum path.

---

# RECURSION DRY RUN

Grid:

| 1 | 3 | 1 |
|---|---|---|
| 1 | 5 | 1 |
| 4 | 2 | 1 |

Need:

```text
solve(2,2)
```

This becomes:

```text
1 + min(
    solve(1,2),
    solve(2,1)
)
```

Again:

```text
solve(1,2)
=
1 + min(
    solve(0,2),
    solve(1,1)
)
```

Recursion continues till:

```text
(0,0)
```

---

# IMPORTANT RECURSION SNIPPETS

## Base Case

```cpp
if(i == 0 && j == 0)
    return grid[0][0];
```

---

## Out of Bounds

```cpp
if(i < 0 || j < 0)
    return 1e9;
```

---

## Transition

```cpp
int up = grid[i][j] + solve(i-1, j);

int left = grid[i][j] + solve(i, j-1);

return min(up, left);
```

---

# RECURSION CODE

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>>& grid){

        // Reached starting cell
        if(i == 0 && j == 0)
            return grid[0][0];

        // Out of bounds
        if(i < 0 || j < 0)
            return 1e9;

        int up = grid[i][j] + solve(i - 1, j, grid);

        int left = grid[i][j] + solve(i, j - 1, grid);

        return min(up, left);
    }

    int minPathSum(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();

        return solve(n - 1, m - 1, grid);
    }
};
```

---

# RECURSION TC & SC

```text
TC:
O(2^(n+m))

SC:
O(n+m)
```

---

# WHY RECURSION IS BAD

Same states repeat.

Example:

```text
solve(1,1)
```

gets called many times.

This is:

```text
Overlapping Subproblems
```

So we use DP.

---

# MEMOIZATION

# MEMOIZATION IDEA

Store already computed answers.

Use:

```cpp
dp[i][j]
```

If answer already exists:

return it directly.

---

# MEMOIZATION DRY RUN

Suppose:

```text
solve(2,2)
```

calls:

```text
solve(1,2)
solve(2,1)
```

Both again call:

```text
solve(1,1)
```

Without DP:

recalculated again.

With memoization:

```cpp
dp[1][1]
```

stored once.

Next time directly returned.

---

# IMPORTANT MEMOIZATION SNIPPETS

## DP Check

```cpp
if(dp[i][j] != -1)
    return dp[i][j];
```

---

## Store Answer

```cpp
return dp[i][j] = min(up, left);
```

---

# MEMOIZATION CODE

```cpp
class Solution {
public:

    int solve(int i, int j,
              vector<vector<int>>& grid,
              vector<vector<int>>& dp){

        if(i == 0 && j == 0)
            return grid[0][0];

        if(i < 0 || j < 0)
            return 1e9;

        if(dp[i][j] != -1)
            return dp[i][j];

        int up = grid[i][j] + solve(i - 1, j, grid, dp);

        int left = grid[i][j] + solve(i, j - 1, grid, dp);

        return dp[i][j] = min(up, left);
    }

    int minPathSum(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> dp(n, vector<int>(m, -1));

        return solve(n - 1, m - 1, grid, dp);
    }
};
```

---

# MEMOIZATION TC & SC

```text
TC:
O(n*m)

SC:
O(n*m) + O(n+m)
```

---

# TABULATION

# TABULATION IDEA

Remove recursion.

Fill DP table iteratively.

Build answers from smaller states.

---

# DP STATE

```cpp
dp[i][j]
=
minimum path sum to reach (i,j)
```

---

# TABULATION DRY RUN

Grid:

| 1 | 3 | 1 |
|---|---|---|
| 1 | 5 | 1 |
| 4 | 2 | 1 |

---

## Start

```text
dp[0][0] = 1
```

---

## First Row

```text
dp[0][1] = 4
dp[0][2] = 5
```

---

## First Column

```text
dp[1][0] = 2
dp[2][0] = 6
```

---

## Remaining

```text
dp[1][1] = 5 + min(4,2) = 7

dp[1][2] = 1 + min(5,7) = 6

dp[2][1] = 2 + min(7,6) = 8

dp[2][2] = 1 + min(6,8) = 7
```

Final Answer:

```text
7
```

---

# IMPORTANT TABULATION SNIPPETS

## Initialize Invalid Paths

```cpp
int up = 1e9;
int left = 1e9;
```

---

# WHY NOT 0?

If:

```cpp
up = 0
```

then:

```cpp
min(0,left)
```

may choose invalid path.

So use:

```text
very large number
```

---

# TRANSITION

```text
dp[i][j]=grid[i][j]+min(dp[i-1][j],dp[i][j-1])
```

---

# TABULATION CODE

```cpp
class Solution {
public:
    
    int minPathSum(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        
        vector<vector<int>> dp(n, vector<int>(m, 0));
        
        for(int i = 0; i < n; i++) {
            
            for(int j = 0; j < m; j++) {
                
                if(i == 0 && j == 0) {
                    dp[i][j] = grid[i][j];
                }
                
                else {
                    
                    int up = 1e9;
                    int left = 1e9;
                    
                    if(i > 0) {
                        up = grid[i][j] + dp[i - 1][j];
                    }
                    
                    if(j > 0) {
                        left = grid[i][j] + dp[i][j - 1];
                    }
                    
                    dp[i][j] = min(up, left);
                }
            }
        }
        
        return dp[n - 1][m - 1];
    }
};
```

---

# TABULATION TC & SC

```text
TC:
O(n*m)

SC:
O(n*m)
```

---

# SPACE OPTIMIZATION

# SPACE OPTIMIZATION IDEA

Observe:

```cpp
dp[i][j]
```

depends only on:

```cpp
dp[i-1][j]
dp[i][j-1]
```

So we only need:

- previous row
- current row

No need entire matrix.

---

# SPACE OPTIMIZATION DRY RUN

At current row:

```cpp
prev[j]
```

represents:

```text
top cell
```

And:

```cpp
curr[j-1]
```

represents:

```text
left cell
```

After finishing row:

```cpp
prev = curr
```

---

# IMPORTANT SPACE OPTIMIZATION SNIPPETS

## Previous Row

```cpp
vector<int> prev(m, 0);
```

---

## Current Row

```cpp
vector<int> curr(m, 0);
```

---

## Top

```cpp
up = grid[i][j] + prev[j];
```

---

## Left

```cpp
left = grid[i][j] + curr[j-1];
```

---

# SPACE OPTIMIZATION CODE

```cpp
class Solution {
public:
    
    int minPathSum(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        
        vector<int> prev(m, 0);
        
        for(int i = 0; i < n; i++) {
            
            vector<int> curr(m, 0);
            
            for(int j = 0; j < m; j++) {
                
                if(i == 0 && j == 0) {
                    curr[j] = grid[i][j];
                }
                
                else {
                    
                    int up = 1e9;
                    int left = 1e9;
                    
                    if(i > 0) {
                        up = grid[i][j] + prev[j];
                    }
                    
                    if(j > 0) {
                        left = grid[i][j] + curr[j - 1];
                    }
                    
                    curr[j] = min(up, left);
                }
            }
            
            prev = curr;
        }
        
        return prev[m - 1];
    }
};
```

---

# SPACE OPTIMIZATION TC & SC

```text
TC:
O(n*m)

SC:
O(m)
```

---

# HOW TO EXPLAIN IN INTERVIEW

# STEP 1

Say:

```text
This is a Grid DP problem.
```

because:

- movement inside grid
- state depends on previous cells

---

# STEP 2

Explain reverse thinking:

```text
To reach (i,j),
I can come from:
top or left
```

---

# STEP 3

Write recurrence:

```text
f(i,j)=grid[i][j]+min(f(i-1,j),f(i,j-1))
```

---

# STEP 4

Explain base cases.

---

# STEP 5

Start with recursion.

Then say:

```text
This recalculates states repeatedly.
```

---

# STEP 6

Move to memoization.

---

# STEP 7

Then say:

```text
We can remove recursion stack
using tabulation.
```

---

# STEP 8

Finally optimize space.

---

# MOST IMPORTANT THING TO REMEMBER

```text
COUNT PATHS:
invalid = 0

MINIMUM PATH:
invalid = large number

MAXIMUM PATH:
invalid = very small number
```

This is the biggest confusion point in grid DP problems.
