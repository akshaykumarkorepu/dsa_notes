
# Maximum Falling Path Sum

# PROBLEM:
Maximum Falling Path Sum

# PATTERN:
2D Grid DP → Variable Start + Variable End

# WHY THIS PATTERN:

- We move on a grid
- We need maximum path
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
"What is the maximum cost to reach this cell?"
```

Possible previous positions:

- Up → `(i-1,j)`
- Left diagonal → `(i-1,j-1)`
- Right diagonal → `(i-1,j+1)`

So:

```text
Current Cell Value
+
maximum of all possible previous paths
```

---

# IMPORTANT OBSERVATION

This problem is NOT always:

```text
n x n
```

Matrix can be:

```text
n x m
```

So ALWAYS use:

```cpp
int n = mat.size();
int m = mat[0].size();
```

---

# VERY IMPORTANT MISTAKES I DID

# MISTAKE 1

Using:

```cpp
j >= n
```

instead of:

```cpp
j >= m
```

Why wrong?

`j` is column index.

So compare with:

```text
number of columns = m
```

NOT rows.

---

# MISTAKE 2

Using:

```cpp
if(j+1 < n)
```

instead of:

```cpp
if(j+1 < m)
```

Why wrong?

Right diagonal checks columns.

So compare with columns count.

---

# MISTAKE 3

In Space Optimization:

Wrong:

```cpp
vector<int> prev(n,0);
vector<int> curr(n,0);
```

Correct:

```cpp
vector<int> prev(m,0);
vector<int> curr(m,0);
```

Why?

Rows contain:

```text
m columns
```

NOT `n`.

---

# MISTAKE 4

Using:

```cpp
-1
```

as memoization marker.

Wrong because actual answer can also become `-1`.

Safer:

```cpp
-1e9
```

---

# SHORTCUT DP NOTES

# DP STATE

```cpp
dp[i][j]
```

means:

```text
Maximum falling path sum
to reach cell (i,j)
from first row
```

---

# TRANSITION

```text
dp[i][j] =
mat[i][j] +
max(
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
if(j < 0 || j >= m)
    return -1e8;
```

Meaning:

```text
Invalid path
```

---

# WHY -1e8 ?

Because this is a MAXIMUM problem.

Invalid paths should NEVER become maximum.

So we return a very small value.

---

# FINAL ANSWER

```text
Maximum among all cells
in last row
```

because ending point is variable.

---

# WHY I MIGHT FORGET

- Forgetting this is VARIABLE START + VARIABLE END
- Forgetting to take maximum from last row
- Forgetting boundary checks for diagonals
- Forgetting why `-1e8` is used
- Using `n` instead of `m`
- Using wrong size in space optimization

---

# HOW TO THINK IN INTERVIEW

Immediately identify:

```text
Grid + Maximum Path + Multiple Moves
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
Maximum falling path sum
to reach cell (i,j)
from first row
```

Then try all 3 possible previous moves.

---

# RECURSION BASE CASES

# 1. OUT OF BOUNDS

```cpp
if(j < 0 || j >= m)
    return -1e8;
```

Why?

Diagonal moves may go outside matrix.

We return very small value so invalid paths never become maximum.

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
if(j < 0 || j >= m)

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
              int n, int m) {
        
        // Out of bounds
        if(j < 0 || j >= m)
            return -1e8;
        
        // First row reached
        if(i == 0)
            return mat[0][j];
        
        
        // Up move
        int up =
            mat[i][j] +
            solve(i-1, j, mat, n, m);
        
        
        // Left diagonal move
        int leftDiagonal =
            mat[i][j] +
            solve(i-1, j-1, mat, n, m);
        
        
        // Right diagonal move
        int rightDiagonal =
            mat[i][j] +
            solve(i-1, j+1, mat, n, m);
        
        
        return max(up,
               max(leftDiagonal,
                   rightDiagonal));
    }
  
  
    int maximumPath(vector<vector<int>>& mat) {
        
        int n = mat.size();
        int m = mat[0].size();
        
        int maxi = -1e8;
        
        
        // Variable ending point
        for(int j = 0; j < m; j++) {
            
            maxi = max(maxi,
                       solve(n-1, j, mat, n, m));
        }
        
        
        return maxi;
    }
};
```

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
Maximum falling path sum
to reach cell (i,j)
```

---

# MEMOIZATION CODE

```cpp
class Solution {
  public:
  
    int solve(int i, int j,
              vector<vector<int>> &mat,
              int n, int m,
              vector<vector<int>> &dp) {
        
        // Out of bounds
        if(j < 0 || j >= m)
            return -1e8;
        
        // First row reached
        if(i == 0)
            return mat[0][j];
        
        
        // Already computed
        if(dp[i][j] != -1e9)
            return dp[i][j];
        
        
        // Up move
        int up =
            mat[i][j] +
            solve(i-1, j, mat, n, m, dp);
        
        
        // Left diagonal move
        int leftDiagonal =
            mat[i][j] +
            solve(i-1, j-1, mat, n, m, dp);
        
        
        // Right diagonal move
        int rightDiagonal =
            mat[i][j] +
            solve(i-1, j+1, mat, n, m, dp);
        
        
        return dp[i][j] =
            max(up,
            max(leftDiagonal,
                rightDiagonal));
    }
  
  
    int maximumPath(vector<vector<int>>& mat) {
        
        int n = mat.size();
        int m = mat[0].size();
        
        vector<vector<int>> dp(
            n,
            vector<int>(m, -1e9)
        );
        
        
        int maxi = -1e8;
        
        
        // Variable ending point
        for(int j = 0; j < m; j++) {
            
            maxi = max(maxi,
                       solve(n-1, j,
                             mat, n, m, dp));
        }
        
        
        return maxi;
    }
};
```

---

# MEMOIZATION TC

```text
O(n*m)
```

---

# MEMOIZATION SC

```text
O(n*m) + O(n)
```

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

# TABULATION CODE

```cpp
class Solution {
  public:
    
    int maximumPath(vector<vector<int>>& mat) {
        
      int n = mat.size();
      int m = mat[0].size();
      
      
      vector<vector<int>> dp(
          n,
          vector<int>(m,0)
      );
      
      
      // Base Case
      for(int j=0;j<m;j++){
          dp[0][j] = mat[0][j];
      }
      
      
      // Remaining rows
      for(int i=1;i<n;i++){
          
          for(int j=0;j<m;j++){
              
              
              // Up move
              int up =
                  mat[i][j] + dp[i-1][j];
              
              
              // Left diagonal move
              int leftDiagonal = mat[i][j];
                
              if(j-1>=0){
                  leftDiagonal += dp[i-1][j-1];
              }
              else{
                  leftDiagonal += -1e8;
              }
                
                
              // Right diagonal move
              int rightDiagonal = mat[i][j];
                
              if(j+1<m){
                  rightDiagonal += dp[i-1][j+1];
              }
              else{
                  rightDiagonal += -1e8;
              }
                
                
              dp[i][j] =
                  max(up,
                  max(leftDiagonal,
                      rightDiagonal));
           }
      }
        
        
      int maxi = -1e8;
        
      for(int j=0;j<m;j++){
          maxi = max(maxi, dp[n-1][j]);
      }
        
        
      return maxi;
    }
};
```

---

# TABULATION TC

```text
O(n*m)
```

---

# TABULATION SC

```text
O(n*m)
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
    
    int maximumPath(vector<vector<int>>& mat) {
        
      int n = mat.size();
      int m = mat[0].size();
      
      
      // Previous row
      vector<int> prev(m,0);
      
      
      // Base Case
      for(int j=0;j<m;j++){
          prev[j] = mat[0][j];
      }
      
      
      // Remaining rows
      for(int i=1;i<n;i++){
          
          vector<int> curr(m,0);
          
          for(int j=0;j<m;j++){
              
              
              // Up move
              int up =
                  mat[i][j] + prev[j];
              
              
              // Left diagonal move
              int leftDiagonal = mat[i][j];
                
              if(j-1>=0){
                  leftDiagonal += prev[j-1];
              }
              else{
                  leftDiagonal += -1e8;
              }
                
                
              // Right diagonal move
              int rightDiagonal = mat[i][j];
                
              if(j+1<m){
                  rightDiagonal += prev[j+1];
              }
              else{
                  rightDiagonal += -1e8;
              }
                
                
              curr[j] =
                  max(up,
                  max(leftDiagonal,
                      rightDiagonal));
           }
           
           
           // Move current row to previous
           prev = curr;
      }
        
        
      int maxi = -1e8;
        
      for(int j=0;j<m;j++){
          maxi = max(maxi, prev[j]);
      }
        
        
      return maxi;
    }
};
```

---

# SPACE OPTIMIZATION TC

```text
O(n*m)
```

---

# SPACE OPTIMIZATION SC

```text
O(m)
```

---

# FINAL INTERVIEW EXPLANATION

```text
This is a Variable Start + Variable End
Grid DP problem.

I define:

dp[i][j] =
maximum path sum to reach cell (i,j)
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
I take maximum from last row.
```

Source reference: :contentReference[oaicite:0]{index=0}
````
