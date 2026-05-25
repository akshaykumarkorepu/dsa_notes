
# NOTE

## PROBLEM:
Minimum Path Sum

---

# PATTERN:
2D Grid DP

---

# WHY THIS PATTERN:

- We move on a grid
- Each cell depends on previous cells
- We need minimum cost/path
- Multiple overlapping subproblems

Classic Grid DP problem.

---

# CORE IDEA:

From every cell `(i,j)`:

We can come from:

- Up → `(i-1,j)`
- Left → `(i,j-1)`

So:

```text
minPath(i,j) =
grid[i][j] +
min(
    minPath(i-1,j),
    minPath(i,j-1)
)
```

---

# SHORTCUT DP NOTES

```text
Move:
Up + Left

Formula:
dp[i][j] =
grid[i][j] + min(up,left)

Base:
dp[0][0] = grid[0][0]

Invalid:
1e9

Why?
Because invalid path should never become minimum.
```

---

# WHY I MIGHT FORGET / GET STUCK

- Forgetting why we use `1e9`
- Using `0` instead of `1e9`
- Confusing counting paths vs minimum path
- Wrong transitions
- Forgetting current cell value addition
- Forgetting base case
- Mixing up row and column

---

# HOW TO THINK IN INTERVIEW

## Step 1
Say:

```text
From each cell I can come from:
1. Up
2. Left
```

---

## Step 2
Say recurrence:

```text
dp(i,j) =
grid[i][j] +
min(dp(i-1,j), dp(i,j-1))
```

---

## Step 3
Discuss base cases:

```text
Out of bounds -> 1e9
Start cell -> grid[0][0]
```

---

## Step 4
Start with recursion

Then optimize:

```text
Recursion
-> Memoization
-> Tabulation
-> Space Optimization
```

---

# RECURSION

# IDEA

We start from destination `(n-1,m-1)`.

At every cell:

- go UP
- go LEFT

Keep exploring until:

- out of bounds
- source reached

Take minimum path.

---

# RECURSIVE FORMULA

```text
f(i,j) =
grid[i][j] +
min(f(i-1,j), f(i,j-1))
```

---

# BASE CASES

```text
1. if(i < 0 || j < 0)
   return 1e9

2. if(i == 0 && j == 0)
   return grid[0][0]
```

---

# IMPORTANT SNIPPETS

## Base Case

```cpp
if(i == 0 && j == 0)
    return grid[0][0];
```

---

## Invalid Path

```cpp
if(i < 0 || j < 0)
    return 1e9;
```

---

## Transition

```cpp
int up = grid[i][j] + solve(i - 1, j, grid);

int left = grid[i][j] + solve(i, j - 1, grid);

return min(up, left);
```

---

# RECURSION CODE

```cpp
class Solution {
public:

    int solve(int i, int j,
              vector<vector<int>>& grid){

        if(i == 0 && j == 0)
            return grid[0][0];

        if(i < 0 || j < 0)
            return 1e9;

        int up = grid[i][j]
               + solve(i - 1, j, grid);

        int left = grid[i][j]
                 + solve(i, j - 1, grid);

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

# RECURSION DRY RUN

Grid:

```text
1 3 1
1 5 1
4 2 1
```

Start:

```text
solve(2,2)
```

Calls:

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

Eventually reaches:

```text
(0,0)
```

Minimum answer becomes:

```text
7
```

---

# TC

```text
O(2^(n+m))
```

---

# SC

```text
O(n+m)
```

Recursion stack depth.

---

# WHY RECURSION IS BAD

Same states repeat.

Example:

```text
solve(1,1)
```

gets calculated many times.

This is:

```text
Overlapping Subproblems
```

So we use DP.

---

# MEMOIZATION

# IDEA

Recursion recalculates same states many times.

Store answers in dp table.

---

# MEMOIZATION FORMULA

```text
dp(i,j) =
grid[i][j] +
min(dp(i-1,j), dp(i,j-1))
```

---

# IMPORTANT SNIPPET

```cpp
if(dp[i][j] != -1)
    return dp[i][j];
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

        int up = grid[i][j]
               + solve(i - 1, j, grid, dp);

        int left = grid[i][j]
                 + solve(i, j - 1, grid, dp);

        return dp[i][j] = min(up, left);
    }

    int minPathSum(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> dp(
            n,
            vector<int>(m, -1)
        );

        return solve(n - 1, m - 1,
                     grid, dp);
    }
};
```

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

# TC

```text
O(n*m)
```

---

# SC

```text
O(n*m) + O(n+m)
```

- DP table
- recursion stack

---

# TABULATION

# IDEA

Convert recursion to iterative DP.

Build answers from smaller states.

---

# TABULATION FLOW

```text
Top Left -> Bottom Right
```

Every cell depends on:

```text
Up + Left
```

---

# IMPORTANT SNIPPETS

## Invalid Paths

```cpp
int up = 1e9;
int left = 1e9;
```

---

# WHY NOT 0?

If:

```cpp
up = 0;
```

Then:

```cpp
min(0,left)
```

may choose invalid path.

Wrong.

So use:

```text
very large number
```

---

# TRANSITION

```text
dp[i][j] =
grid[i][j] + min(up,left)
```

---

# TABULATION CODE

```cpp
class Solution {
public:
    
    int minPathSum(vector<vector<int>>& grid) {
        
        int n = grid.size();
        int m = grid[0].size();
        
        vector<vector<int>> dp(
            n,
            vector<int>(m, 0)
        );
        
        for(int i = 0; i < n; i++) {
            
            for(int j = 0; j < m; j++) {
                
                if(i == 0 && j == 0) {
                    dp[i][j] = grid[i][j];
                }
                
                else {
                    
                    int up = 1e9;
                    int left = 1e9;
                    
                    if(i > 0)
                        up = grid[i][j]
                           + dp[i - 1][j];
                    
                    if(j > 0)
                        left = grid[i][j]
                             + dp[i][j - 1];
                    
                    dp[i][j] = min(up, left);
                }
            }
        }
        
        return dp[n - 1][m - 1];
    }
};
```

---

# TABULATION DRY RUN

Grid:

```text
1 3 1
1 5 1
4 2 1
```

DP:

```text
1 4 5
2 7 6
6 8 7
```

Answer:

```text
7
```

---

# TC

```text
O(n*m)
```

---

# SC

```text
O(n*m)
```

---

# SPACE OPTIMIZATION

# IDEA

Current row only depends on:

- previous row
- current row left value

So we do not need entire matrix.

Use:

```text
prev[]
curr[]
```

---

# IMPORTANT OBSERVATION

```text
up    -> prev[j]
left  -> curr[j-1]
```

---

# SPACE OPTIMIZED CODE

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
                    
                    if(i > 0)
                        up = grid[i][j]
                           + prev[j];
                    
                    if(j > 0)
                        left = grid[i][j]
                             + curr[j - 1];
                    
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

# SPACE OPTIMIZATION DRY RUN

After Row 0:

```text
prev = [1 4 5]
```

After Row 1:

```text
prev = [2 7 6]
```

After Row 2:

```text
prev = [6 8 7]
```

Answer:

```text
7
```

---

# TC

```text
O(n*m)
```

---

# SC

```text
O(m)
```

---

# INTERVIEW FLOW

If interviewer asks:

```text
Minimum Path Sum
```

You should say:

---

## STEP 1

```text
This is a Grid DP problem.
Each cell depends on:
1. Up
2. Left
```

---

## STEP 2

State recurrence:

```text
dp(i,j) =
grid[i][j] +
min(dp(i-1,j), dp(i,j-1))
```

---

## STEP 3

Mention base cases:

```text
Out of bounds -> 1e9
Start cell -> grid[0][0]
```

---

## STEP 4

Say optimization flow:

```text
Recursion
-> Memoization
-> Tabulation
-> Space Optimization
```

---

# FINAL MEMORY TRICK

```text
GRID PATH QUESTIONS:

Count ways:
UP + LEFT

Minimum path:
MIN(UP, LEFT)

Maximum path:
MAX(UP, LEFT)
```
````
