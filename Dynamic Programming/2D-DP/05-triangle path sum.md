# NOTE

# PROBLEM:
Triangle Minimum Path Sum

# PATTERN:
2D DP on Grids / Triangle DP

# WHY THIS PATTERN:
At every cell we have multiple choices and we need:

- minimum path
- optimal answer
- overlapping subproblems

Current state depends on future states.

---

# QUESTION UNDERSTANDING

We are given a triangle.

Example:

```text
      2
     3 4
    6 5 7
   4 1 8 3
```

We start from TOP and move till BOTTOM.

From every cell we can move only:

- Down      -> `(i+1, j)`
- Diagonal  -> `(i+1, j+1)`

We need:

```text
minimum path sum
```

---

# VERY IMPORTANT OBSERVATIONS

## 1. We START from TOP

We start from:

```cpp
(0,0)
```

because top is FIXED.

There is only ONE starting point.

---

## 2. We DO NOT start from bottom

Bottom row contains multiple cells:

```text
4 1 8 3
```

These are VARIABLE ending points.

If we start from bottom:

- which one do we choose?
- 4?
- 1?
- 8?
- 3?

This creates unnecessary complexity.

So we naturally start from TOP.

---

# 3. NO BOUNDARY CHECKS NEEDED

This is VERY IMPORTANT.

Triangle property:

```text
triangle[i].size() = triangle[i-1].size() + 1
```

Meaning:

Every next row has exactly one extra element.

So from index:

```text
j
```

we can ALWAYS go to:

```text
j
j+1
```

Therefore these are ALWAYS valid:

```cpp
solve(i+1,j)

solve(i+1,j+1)
```

No out-of-bound conditions needed.

---

# SHORTCUT DP NOTES

## State

```cpp
solve(i,j)
```

Meaning:

```text
Minimum path sum starting from (i,j) till bottom.
```

---

## Choices

From every cell:

```text
1. Down
2. Diagonal
```

---

## Recurrence

:contentReference[oaicite:0]{index=0}

---

## Base Case

When we reach last row:

```cpp
if(i == n-1)
    return triangle[i][j];
```

Because from last row there is nowhere to go.

---

# WHY I MIGHT FORGET

- Why no boundary checks needed
- Why we start from top and not bottom
- Why tabulation starts bottom-up
- Why only previous row is needed in space optimization
- `triangle[n-1]` gives complete last row vector
- `front = triangle[n-1]` copies the last row

---

# RECURSION

# IDEA

From every cell:

- move down
- move diagonal

Take minimum.

---

# RECURSION CODE

```cpp
class Solution {
public:

    int solve(int i, int j,
              vector<vector<int>>& triangle,
              int n){

        // Base Case
        if(i == n-1)
            return triangle[i][j];

        int down =
        triangle[i][j] +
        solve(i+1, j, triangle, n);

        int diagonal =
        triangle[i][j] +
        solve(i+1, j+1, triangle, n);

        return min(down, diagonal);
    }

    int minPathSum(vector<vector<int>>& triangle) {

        int n = triangle.size();

        return solve(0,0,triangle,n);
    }
};
```

---

# RECURSION DRY RUN

Triangle:

```text
      2
     3 4
    6 5 7
   4 1 8 3
```

Start:

```cpp
solve(0,0)
```

---

## solve(0,0)

```cpp
2 + min(
    solve(1,0),
    solve(1,1)
)
```

---

## solve(1,0)

```cpp
3 + min(
    solve(2,0),
    solve(2,1)
)
```

---

## solve(2,0)

```cpp
6 + min(
    solve(3,0),
    solve(3,1)
)
```

Last row:

```cpp
solve(3,0)=4
solve(3,1)=1
```

So:

```cpp
6 + min(4,1)
= 7
```

---

## solve(2,1)

```cpp
5 + min(1,8)
= 6
```

---

## solve(1,0)

```cpp
3 + min(7,6)
= 9
```

---

## solve(1,1)

```cpp
4 + min(6,10)
= 10
```

---

## solve(0,0)

```cpp
2 + min(9,10)
= 11
```

Answer:

```text
11
```

Path:

```text
2 → 3 → 5 → 1
```

---

# RECURSION TC

```text
O(2^n)
```

Why?

At every cell:

```text
2 recursive calls
```

forming exponential recursion tree.

---

# RECURSION SC

```text
O(n)
```

Reason:

Maximum recursion depth = number of rows.

---

# MEMOIZATION

# IDEA

Recursion recalculates same states again and again.

Example:

```cpp
solve(2,1)
```

can come from multiple paths.

So we store already computed answers.

---

# DP INITIALIZATION

```cpp
vector<vector<int>> dp(n, vector<int>(n,-1));
```

Meaning:

```text
-1 = not computed yet
```

---

# MEMOIZATION CODE

```cpp
class Solution {
public:

    int solve(int i, int j,
              vector<vector<int>>& triangle,
              int n,
              vector<vector<int>>& dp){

        if(i == n-1)
            return triangle[i][j];

        if(dp[i][j] != -1)
            return dp[i][j];

        int down =
        triangle[i][j] +
        solve(i+1,j,triangle,n,dp);

        int diagonal =
        triangle[i][j] +
        solve(i+1,j+1,triangle,n,dp);

        return dp[i][j] =
        min(down, diagonal);
    }

    int minPathSum(vector<vector<int>>& triangle) {

        int n = triangle.size();

        vector<vector<int>> dp(n,
                               vector<int>(n,-1));

        return solve(0,0,triangle,n,dp);
    }
};
```

---

# MEMOIZATION TC

```text
O(n^2)
```

Why?

Each state:

```cpp
(i,j)
```

computed only once.

Total states ≈ triangle cells ≈ `n²`

---

# MEMOIZATION SC

DP array:

```text
O(n^2)
```

Recursion stack:

```text
O(n)
```

Total:

```text
O(n^2)
```

---

# TABULATION

# CORE IDEA

Recursion computes answers:

```text
bottom → upward
```

during backtracking.

Tabulation directly computes:

```text
bottom → top
```

using loops.

---

# WHY BOTTOM-UP?

Recurrence:

:contentReference[oaicite:1]{index=1}

Current row depends on NEXT row.

So next row must already be computed.

Hence:

```text
bottom-up
```

---

# TABULATION FLOW

## Step 1

Create DP table.

```cpp
vector<vector<int>> dp(n, vector<int>(n,0));
```

---

## Step 2

Copy last row.

```cpp
for(int j=0; j<n; j++){

    dp[n-1][j] = triangle[n-1][j];
}
```

Last row already contains correct answers.

---

## Step 3

Move upward.

```cpp
for(int i=n-2; i>=0; i--)
```

---

## Step 4

Traverse current row.

```cpp
for(int j=i; j>=0; j--)
```

Why?

Row `i` has:

```text
i+1 elements
```

So valid columns:

```text
0 to i
```

---

# TABULATION DRY RUN

Initial DP:

```text
      -
     - -
    - - -
   4 1 8 3
```

---

## Row 2

```text
6 5 7
```

For 6:

```cpp
6 + min(4,1)
= 7
```

For 5:

```cpp
5 + min(1,8)
= 6
```

For 7:

```cpp
7 + min(8,3)
= 10
```

DP:

```text
      -
     - -
    7 6 10
   4 1 8 3
```

---

## Row 1

```text
3 4
```

For 3:

```cpp
3 + min(7,6)
= 9
```

For 4:

```cpp
4 + min(6,10)
= 10
```

DP:

```text
      -
     9 10
    7 6 10
   4 1 8 3
```

---

## Row 0

```cpp
2 + min(9,10)
= 11
```

Final Answer:

```cpp
dp[0][0]
```

---

# TABULATION CODE

```cpp
class Solution {
public:

    int minPathSum(vector<vector<int>>& triangle) {

        int n = triangle.size();

        vector<vector<int>> dp(n,
                               vector<int>(n,0));

        // Copy last row
        for(int j=0; j<n; j++){

            dp[n-1][j] = triangle[n-1][j];
        }

        // Bottom-up
        for(int i=n-2; i>=0; i--){

            for(int j=i; j>=0; j--){

                int down =
                triangle[i][j] + dp[i+1][j];

                int diagonal =
                triangle[i][j] + dp[i+1][j+1];

                dp[i][j] =
                min(down, diagonal);
            }
        }

        return dp[0][0];
    }
};
```

---

# TABULATION TC

```text
O(n^2)
```

Every triangle cell visited once.

---

# TABULATION SC

```text
O(n^2)
```

DP table used.

---

# SPACE OPTIMIZATION

# CORE IDEA

Current row depends ONLY on next row.

So entire DP table is unnecessary.

Store only:

```text
previous row
```

---

# VERY IMPORTANT

This:

```cpp
vector<int> front = triangle[n-1];
```

is equivalent to:

```cpp
vector<int> front(n);

for(int j=0; j<n; j++){

    front[j] = triangle[n-1][j];
}
```

Both are SAME.

---

# WHY?

Because:

```cpp
triangle[n-1]
```

returns entire last row vector.

Example:

```cpp
triangle[3]
=
{4,1,8,3}
```

---

# SPACE OPTIMIZATION FLOW

## front

Stores:

```text
answers of lower row
```

---

## curr

Stores:

```text
answers of current row
```

---

# SPACE OPTIMIZED CODE

```cpp
class Solution {
public:

    int minPathSum(vector<vector<int>>& triangle) {

        int n = triangle.size();

        vector<int> front = triangle[n-1];

        for(int i=n-2; i>=0; i--){

            vector<int> curr(n,0);

            for(int j=i; j>=0; j--){

                int down =
                triangle[i][j] + front[j];

                int diagonal =
                triangle[i][j] + front[j+1];

                curr[j] =
                min(down, diagonal);
            }

            front = curr;
        }

        return front[0];
    }
};
```

---

# SPACE OPTIMIZATION DRY RUN

Initial:

```text
front = [4,1,8,3]
```

---

## Row 2

```text
6 5 7
```

Compute:

```text
7 6 10
```

Now:

```text
front = [7,6,10]
```

---

## Row 1

```text
3 4
```

Compute:

```text
9 10
```

Now:

```text
front = [9,10]
```

---

## Row 0

```text
2 + min(9,10)
= 11
```

Final:

```cpp
front[0]
```

---

# SPACE OPTIMIZATION TC

```text
O(n^2)
```

---

# SPACE OPTIMIZATION SC

```text
O(n)
```

Only two 1D arrays used.

---

# INTERVIEW EXPLANATION FLOW

## Recursion

> “From every cell I have two choices:
> down and diagonal.
>
> So I define a recursive function `solve(i,j)`
> which returns minimum path sum from that cell till bottom.”

---

## Why No Boundary Checks

> “Triangle structure guarantees valid adjacent cells,
> so `(j)` and `(j+1)` always exist in next row.”

---

## Why Start From Top

> “Top is a fixed starting point,
> whereas bottom contains multiple possible ending points.”

---

## Memoization

> “Many states repeat,
> so I store already computed answers in DP.”

---

## Tabulation

> “Current row depends on next row,
> so I reverse recursion and compute bottom-up.”

---

## Space Optimization

> “Since current row depends only on lower row,
> I store only one previous row instead of entire DP table.”

---

# FINAL COMPLEXITIES

| Approach | TC | SC |
|---|---|---|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n^2) | O(n^2) |
| Tabulation | O(n^2) | O(n^2) |
| Space Optimization | O(n^2) | O(n) |
