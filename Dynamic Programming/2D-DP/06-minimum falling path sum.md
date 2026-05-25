
# Minimum Falling Path Sum

# PROBLEM:
Minimum Falling Path Sum

# PATTERN:
2D Grid DP → Variable Start + Variable End

# WHY THIS PATTERN:

- We move on a grid
- We need minimum path
- Multiple movement choices
- Start can be from ANY column in first row
- End can be at ANY column in last row

So this becomes:

```text
Variable Start + Variable End Grid DP
```

---

# CORE IDEA:

For every cell `(i,j)`:

We ask:

```text
"What is the minimum cost to reach this cell?"
```

Possible previous positions:

- Up → `(i-1,j)`
- Left diagonal → `(i-1,j-1)`
- Right diagonal → `(i-1,j+1)`

So:

```text
Current Cell Value
+
minimum of all possible previous paths
```

---

# SHORTCUT DP NOTES

# DP STATE

```cpp
dp[i][j]
```

means:

```text
Minimum falling path sum
to reach cell (i,j)
from first row
```

---

# TRANSITION

```text
dp[i][j] =
mat[i][j] +
min(
    up,
    leftDiagonal,
    rightDiagonal
)
```

---

# BASE CASE

```cpp
if(i == 0)
    return mat[0][j];
```

Meaning:

```text
First row itself is the starting row
```

---

# BOUNDARY CHECK

```cpp
if(j < 0 || j >= n)
    return 1e9;
```

Meaning:

```text
Invalid path
```

---

# FINAL ANSWER

```text
Minimum among all cells
in last row
```

because ending point is variable.

---

# WHY I MIGHT FORGET

- Forgetting this is VARIABLE START + VARIABLE END
- Forgetting to take minimum from last row
- Forgetting boundary checks for diagonals
- Forgetting why `1e9` is used
- Confusing movement direction

---

# HOW TO THINK IN INTERVIEW

Immediately identify:

```text
Grid + Minimum Path + Multiple Moves
```

Then ask:

```text
Fixed start/end OR variable start/end?
```

Here:

```text
Start = variable
End = variable
```

So:

- Base case handles variable start
- Final loop handles variable end

---

# RECURSION

# IDEA

Define:

```cpp
f(i,j)
```

as:

```text
Minimum falling path sum
to reach cell (i,j)
from first row
```

Then try all 3 possible previous moves.

---

# RECURSION BASE CASES

# 1. OUT OF BOUNDS

```cpp
if(j < 0 || j >= n)
    return 1e9;
```

Why?

Diagonal moves may go outside matrix.

We return huge value so invalid paths never become minimum.

---

# 2. FIRST ROW

```cpp
if(i == 0)
    return mat[0][j];
```

Meaning:

Reached starting row.

---

# IMPORTANT ORDER

Always:

```cpp
if(j < 0 || j >= n)

if(i == 0)
```

Boundary check MUST come first.

Otherwise:

```cpp
mat[0][-1]
```

causes runtime error.

---

# RECURSION CODE

```cpp
class Solution {
  public:
  
    int solve(int i, int j,
              vector<vector<int>> &mat,
              int n) {
        
        // Out of bounds
        if(j < 0 || j >= n)
            return 1e9;
        
        // First row reached
        if(i == 0)
            return mat[0][j];
        
        
        // Up move
        int up =
            mat[i][j] +
            solve(i-1, j, mat, n);
        
        
        // Left diagonal move
        int leftDiagonal =
            mat[i][j] +
            solve(i-1, j-1, mat, n);
        
        
        // Right diagonal move
        int rightDiagonal =
            mat[i][j] +
            solve(i-1, j+1, mat, n);
        
        
        return min(up,
               min(leftDiagonal,
                   rightDiagonal));
    }
  
  
    int minFallingPathSum(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        int mini = 1e9;
        
        
        // Variable ending point
        for(int j = 0; j < n; j++) {
            
            mini = min(mini,
                       solve(n-1, j, mat, n));
        }
        
        
        return mini;
    }
};
```

---

# RECURSION DRY RUN

Matrix:

```text
1 2 3
4 5 6
7 8 9
```

Suppose:

```cpp
solve(2,1)
```

meaning:

```text
Minimum path to reach 8
```

Possible moves:

```text
8 + solve(1,1)
8 + solve(1,0)
8 + solve(1,2)
```

Further expands recursively.

Eventually reaches first row.

---

# RECURSION TC

```text
O(3^n)
```

---

# RECURSION SC

```text
O(n)
```

Recursion stack depth.

---

# MEMOIZATION

# IDEA

Recursion recomputes same states again and again.

Store answers in DP table.

---

# DP STATE

```cpp
dp[i][j]
```

means:

```text
Minimum falling path sum
to reach cell (i,j)
```

---

# MEMOIZATION CODE

```cpp
class Solution {
  public:
  
    int solve(int i, int j,
              vector<vector<int>> &mat,
              int n,
              vector<vector<int>> &dp) {
        
        // Out of bounds
        if(j < 0 || j >= n)
            return 1e9;
        
        // First row reached
        if(i == 0)
            return mat[0][j];
        
        
        // Already computed
        if(dp[i][j] != -1)
            return dp[i][j];
        
        
        // Up move
        int up =
            mat[i][j] +
            solve(i-1, j, mat, n, dp);
        
        
        // Left diagonal move
        int leftDiagonal =
            mat[i][j] +
            solve(i-1, j-1, mat, n, dp);
        
        
        // Right diagonal move
        int rightDiagonal =
            mat[i][j] +
            solve(i-1, j+1, mat, n, dp);
        
        
        return dp[i][j] =
            min(up,
            min(leftDiagonal,
                rightDiagonal));
    }
  
  
    int minFallingPathSum(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        vector<vector<int>> dp(
            n,
            vector<int>(n, -1)
        );
        
        
        int mini = 1e9;
        
        
        // Variable ending point
        for(int j = 0; j < n; j++) {
            
            mini = min(mini,
                       solve(n-1, j,
                             mat, n, dp));
        }
        
        
        return mini;
    }
};
```

---

# MEMOIZATION DRY RUN

Suppose:

```text
solve(2,1)
```

calls:

```text
solve(1,1)
```

Another branch ALSO calls:

```text
solve(1,1)
```

Without memoization:

```text
Computed again
```

With memoization:

```cpp
if(dp[i][j] != -1)
```

stored answer returned instantly.

---

# MEMOIZATION TC

```text
O(n*n)
```

---

# MEMOIZATION SC

```text
O(n*n) + O(n)
```

DP table + recursion stack.

---

# TABULATION

# IDEA

Convert recursion into iterative DP.

Fill DP table row by row.

---

# HOW TO FIND BASE CASE

Take recursive base case:

```cpp
if(i == 0)
```

Convert into DP initialization:

```cpp
dp[0][j]
```

---

# TABULATION DP STATE

```cpp
dp[i][j]
```

means:

```text
Minimum path sum to reach cell (i,j)
```

---

# TRANSITION

```text
Current Cell
+
minimum of previous 3 paths
```

---

# TABULATION CODE

```cpp
class Solution {
  public:
  
    int minFallingPathSum(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        vector<vector<int>> dp(
            n,
            vector<int>(n, 0)
        );
        
        
        // Base Case
        for(int j = 0; j < n; j++) {
            dp[0][j] = mat[0][j];
        }
        
        
        // Fill remaining rows
        for(int i = 1; i < n; i++) {
            
            for(int j = 0; j < n; j++) {
                
                
                // up move
                int up =
                    mat[i][j] + dp[i-1][j];
                
                
                // left diagonal move
                int leftDiagonal = mat[i][j];
                
                // if left diagonal exists
                if(j-1 >= 0)
                    leftDiagonal += dp[i-1][j-1];
                
                // otherwise invalid path
                else
                    leftDiagonal += 1e9;
                
                
                // right diagonal move
                int rightDiagonal = mat[i][j];
                
                // if right diagonal exists
                if(j+1 < n)
                    rightDiagonal += dp[i-1][j+1];
                
                // otherwise invalid path
                else
                    rightDiagonal += 1e9;
                
                
                // store minimum path
                dp[i][j] =
                    min(up,
                    min(leftDiagonal,
                        rightDiagonal));
            }
        }
        
        
        // Variable ending point
        int mini = 1e9;
        
        for(int j = 0; j < n; j++) {
            mini = min(mini, dp[n-1][j]);
        }
        
        
        return mini;
    }
};
```

---

# TABULATION DRY RUN

Initial DP:

```text
1 2 3
0 0 0
0 0 0
```

Compute second row:

```text
5 6 8
```

Compute third row:

```text
12 13 15
```

Answer:

```text
min(12,13,15) = 12
```

---

# TABULATION TC

```text
O(n*n)
```

---

# TABULATION SC

```text
O(n*n)
```

---

# SPACE OPTIMIZATION

# IDEA

Current row depends ONLY on previous row.

So entire DP matrix unnecessary.

Store only:

```text
prev row
curr row
```

---

# SPACE OPTIMIZED CODE

```cpp
class Solution {
  public:
  
    int minFallingPathSum(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        
        // First row
        vector<int> prev(n, 0);
        
        for(int j = 0; j < n; j++) {
            prev[j] = mat[0][j];
        }
        
        
        // Remaining rows
        for(int i = 1; i < n; i++) {
            
            vector<int> curr(n, 0);
            
            for(int j = 0; j < n; j++) {
                
                
                // up move
                int up =
                    mat[i][j] + prev[j];
                
                
                // left diagonal move
                int leftDiagonal = mat[i][j];
                
                if(j-1 >= 0)
                    leftDiagonal += prev[j-1];
                else
                    leftDiagonal += 1e9;
                
                
                // right diagonal move
                int rightDiagonal = mat[i][j];
                
                if(j+1 < n)
                    rightDiagonal += prev[j+1];
                else
                    rightDiagonal += 1e9;
                
                
                curr[j] =
                    min(up,
                    min(leftDiagonal,
                        rightDiagonal));
            }
            
            
            // Move current row to prev
            prev = curr;
        }
        
        
        // Variable ending point
        int mini = 1e9;
        
        for(int j = 0; j < n; j++) {
            mini = min(mini, prev[j]);
        }
        
        
        return mini;
    }
};
```

---

# SPACE OPTIMIZATION DRY RUN

Initially:

```text
prev = [1 2 3]
```

After second row:

```text
curr = [5 6 8]
```

Now:

```cpp
prev = curr
```

So:

```text
prev = [5 6 8]
```

Continue similarly.

---

# SPACE OPTIMIZATION TC

```text
O(n*n)
```

---

# SPACE OPTIMIZATION SC

```text
O(n)
```

---

# FINAL INTERVIEW EXPLANATION

```text
This is a Variable Start + Variable End
Grid DP problem.

I define:

dp[i][j] =
minimum path sum to reach cell (i,j)
from first row.

Each state depends on:
up, left diagonal, and right diagonal.

The recursive base case:
i == 0
becomes initialization of first row
in tabulation.

Since current row depends only
on previous row,
space optimization is possible
using prev and curr arrays.

Finally,
because ending point is variable,
I take minimum from last row.
```
````
