
# NOTE

## PROBLEM:
Unique Paths in a Grid with Obstacles

---

# PATTERN:
2D Grid DP

---

# WHY THIS PATTERN:

- We move on a grid
- Each cell depends on previous cells
- We need count of ways
- Multiple overlapping subproblems

Classic DP on Grid problem.

---

# CORE IDEA:

From every cell `(i,j)`:

We can come from:

- Up → `(i-1,j)`
- Left → `(i,j-1)`

So:

```text
ways(i,j) = ways(i-1,j) + ways(i,j-1)
```

BUT:

- If obstacle → return 0
- If out of bounds → return 0
- If source reached → return 1

---

# SHORTCUT DP NOTES

```text
Move:
Up + Left

Formula:
dp[i][j] = dp[i-1][j] + dp[i][j-1]

Obstacle:
0 ways

Base:
dp[0][0] = 1
```

---

# WHY I MIGHT FORGET / GET STUCK

- Forgetting obstacle check BEFORE base case
- Confusing directions
- Using n instead of m
- Wrong dp dimensions
- Forgetting curr should be outside inner loop
- Returning too early inside loops

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
ways(i,j) = ways(i-1,j) + ways(i,j-1)
```

---

## Step 3
Discuss base cases:

```text
Out of bounds -> 0
Obstacle -> 0
Start cell -> 1
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

- obstacle
- out of bounds
- source reached

---

# RECURSIVE FORMULA

```text
f(i,j) = f(i-1,j) + f(i,j-1)
```

---

# BASE CASES

```text
1. if(i < 0 || j < 0) return 0

2. if(grid[i][j] == 1) return 0

3. if(i == 0 && j == 0) return 1
```

---

# RECURSION CODE

```cpp
class Solution {
public:

    int solve(int i, int j, vector<vector<int>> &grid) {

        if(i < 0 || j < 0)
            return 0;

        if(grid[i][j] == 1)
            return 0;

        if(i == 0 && j == 0)
            return 1;

        int up = solve(i - 1, j, grid);

        int left = solve(i, j - 1, grid);

        return up + left;
    }

    int uniquePaths(vector<vector<int>> &grid) {

        int n = grid.size();
        int m = grid[0].size();

        return solve(n - 1, m - 1, grid);
    }
};
```

---

# DRY RUN

Grid:

```text
0 0 0
0 1 0
0 0 0
```

Start:

```text
solve(2,2)
```

Calls:

```text
solve(1,2) + solve(2,1)
```

Obstacle at `(1,1)` returns `0`.

Valid paths contribute `1`.

Final answer:

```text
2
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

# MEMOIZATION

# IDEA

Recursion recalculates same states many times.

Store answers in dp table.

---

# MEMOIZATION FORMULA

```text
dp(i,j) = dp(i-1,j) + dp(i,j-1)
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
              vector<vector<int>> &grid,
              vector<vector<int>> &dp) {

        if(i < 0 || j < 0)
            return 0;

        if(grid[i][j] == 1)
            return 0;

        if(i == 0 && j == 0)
            return 1;

        if(dp[i][j] != -1)
            return dp[i][j];

        int up = solve(i - 1, j, grid, dp);

        int left = solve(i, j - 1, grid, dp);

        return dp[i][j] = up + left;
    }

    int uniquePaths(vector<vector<int>> &grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> dp(n,
        vector<int>(m, -1));

        return solve(n - 1, m - 1,
                     grid, dp);
    }
};
```

---

# DRY RUN

Suppose:

```text
solve(2,2)
```

Later:

```text
solve(1,2)
```

Again another path asks:

```text
solve(1,2)
```

This time:

```cpp
return dp[1][2];
```

No recalculation.

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

# IMPORTANT SNIPPET

```cpp
dp[i][j] = up + left;
```

---

# TABULATION CODE

```cpp
class Solution {
public:

    int uniquePaths(vector<vector<int>> &grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> dp(n,
        vector<int>(m, 0));

        for(int i = 0; i < n; i++) {

            for(int j = 0; j < m; j++) {

                if(grid[i][j] == 1) {
                    dp[i][j] = 0;
                }

                else if(i == 0 && j == 0) {
                    dp[i][j] = 1;
                }

                else {

                    int up = 0;
                    int left = 0;

                    if(i > 0)
                        up = dp[i - 1][j];

                    if(j > 0)
                        left = dp[i][j - 1];

                    dp[i][j] = up + left;
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
0 0 0
0 1 0
0 0 0
```

DP:

```text
1 1 1
1 0 1
1 1 2
```

Answer:

```text
2
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

    int uniquePaths(vector<vector<int>> &grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<int> prev(m, 0);

        for(int i = 0; i < n; i++) {

            vector<int> curr(m, 0);

            for(int j = 0; j < m; j++) {

                if(grid[i][j] == 1) {
                    curr[j] = 0;
                }

                else if(i == 0 && j == 0) {
                    curr[j] = 1;
                }

                else {

                    int up = 0;
                    int left = 0;

                    if(i > 0)
                        up = prev[j];

                    if(j > 0)
                        left = curr[j - 1];

                    curr[j] = up + left;
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
prev = [1 1 1]
```

After Row 1:

```text
prev = [1 0 1]
```

After Row 2:

```text
prev = [1 1 2]
```

Answer:

```text
2
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
Unique Paths with Obstacles
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
dp(i,j) = dp(i-1,j) + dp(i,j-1)
```

---

## STEP 3

Mention base cases:

```text
Obstacle -> 0
Out of bounds -> 0
Start cell -> 1
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
