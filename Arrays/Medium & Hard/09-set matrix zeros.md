# Set Matrix Zeroes

---

## PROBLEM:

Given an `n × m` matrix, if any cell contains `0`, modify the matrix so that its **entire row and entire column become 0**.

> **Important:** Only the **original zeros** should determine which rows and columns become zero. Newly created zeros should **not** affect the answer.

---

## PATTERN:

**Matrix Marking / In-place Markers**

---

## WHY THIS PATTERN:

The moment you convert a row or column to zero, you create **new zeros**.

If you continue traversing normally, these newly created zeros will also start affecting other rows and columns, producing an incorrect answer.

Therefore,

- First **mark** which rows and columns need to become zero.
- Then **modify** the matrix in another pass.

This is the classic **"Mark First, Modify Later"** pattern.

---

## CORE IDEA:

The solution naturally evolves through **three approaches**.

### Method 1

Store every original zero position.

### Method 2

Instead of storing positions, store which rows and columns should become zero.

### Method 3

Instead of creating two arrays, use the **first row** and **first column** themselves as the marker arrays.

---

# BRUTE FORCE

## Method 1 — Store All Zero Positions

### Intuition

Whenever you find a zero,

don't modify the matrix immediately.

Instead,

store its coordinates.

```cpp
vector<pair<int,int>> zeros;
```

After scanning the entire matrix,

for every stored position,

- make its row zero
- make its column zero

This ensures only the **original zeros** are used.

---

### Algorithm

### Pass 1

Store all zero positions.

```cpp
vector<pair<int,int>> zeros;

for(int i=0;i<n;i++){
    for(int j=0;j<m;j++){

        if(mat[i][j]==0)
            zeros.push_back({i,j});
    }
}
```

---

### Pass 2

For every stored coordinate,

- Zero the entire row.
- Zero the entire column.

---

### Dry Run

Input

```
1 1 1
1 0 1
1 1 1
```

Store

```
zeros = {(1,1)}
```

Zero row

```
1 1 1
0 0 0
1 1 1
```

Zero column

```
1 0 1
0 0 0
1 0 1
```

Correct Answer.

---

### Code

```cpp
class Solution {
public:
    void setMatrixZeroes(vector<vector<int>> &mat) {

        int n = mat.size();
        int m = mat[0].size();

        vector<pair<int,int>> zeros;

        // Store all original zero positions
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){

                if(mat[i][j]==0)
                    zeros.push_back({i,j});
            }
        }

        // Zero corresponding rows and columns
        for(auto &p:zeros){

            int row=p.first;
            int col=p.second;

            for(int j=0;j<m;j++)
                mat[row][j]=0;

            for(int i=0;i<n;i++)
                mat[i][col]=0;
        }
    }
};
```

---

### Time Complexity

Scanning matrix

```
O(nm)
```

Suppose there are **k** zeros.

For every zero,

```
Zero row → O(m)

Zero column → O(n)
```

Total

```
O(nm + k(n+m))
```

Worst Case

```
k = nm

O(nm(n+m))
```

---

### Space Complexity

Store every zero position.

Worst Case

```
O(nm)
```

---

## Transition to Better Approach

Notice something.

Do we really need to remember

```
(1,2)

(3,5)

(7,1)
```

Not really.

We only need to know

```
Row 1 becomes zero

Row 3 becomes zero

Column 2 becomes zero

Column 5 becomes zero
```

So instead of storing positions,

store rows and columns.

---

# BETTER APPROACH

## Method 2 — Two Marker Arrays

### Intuition

Create

```cpp
vector<int> row(n,0);
vector<int> col(m,0);
```

Whenever

```cpp
mat[i][j]==0
```

Mark

```cpp
row[i]=1;
col[j]=1;
```

Later,

if either

```cpp
row[i]==1
```

or

```cpp
col[j]==1
```

make the current cell zero.

---

### Algorithm

### Pass 1

Mark rows and columns.

```
row

0 1 0

col

0 1 0
```

---

### Pass 2

```
if(row[i] || col[j])

mat[i][j]=0;
```

---

### Dry Run

Input

```
1 1 1
1 0 1
1 1 1
```

After marking

```
row

0 1 0

col

0 1 0
```

Second pass

```
1 0 1
0 0 0
1 0 1
```

---

### Code

```cpp
class Solution {
public:

    void setMatrixZeroes(vector<vector<int>> &mat) {

        int n=mat.size();
        int m=mat[0].size();

        vector<int> row(n,0);
        vector<int> col(m,0);

        // Mark rows and columns
        for(int i=0;i<n;i++){

            for(int j=0;j<m;j++){

                if(mat[i][j]==0){

                    row[i]=1;
                    col[j]=1;
                }
            }
        }

        // Update matrix
        for(int i=0;i<n;i++){

            for(int j=0;j<m;j++){

                if(row[i] || col[j])
                    mat[i][j]=0;
            }
        }
    }
};
```

---

### Time Complexity

```
O(nm)
```

---

### Space Complexity

```
row[] = O(n)

col[] = O(m)

Total = O(n+m)
```

---

## Transition to Optimal

Observe something.

The arrays

```
row[]

col[]
```

are only storing markers.

Can we avoid creating them?

Yes.

The matrix already has

```
First Row

First Column
```

We can reuse them.

This gives us

```
O(1)
```

extra space.

---

# OPTIMAL APPROACH

### Idea

Use

```
First Column

↓

Store Row Markers
```

Use

```
First Row

↓

Store Column Markers
```

The only issue is

```
mat[0][0]
```

belongs to

- First Row
- First Column

at the same time.

Therefore,

introduce

```cpp
int col0=1;
```

to separately remember whether the **first column** should become zero.

---

## ALGORITHM

### Step 1 — Mark Rows & Columns

Traverse the matrix.

Whenever

```cpp
mat[i][j]==0
```

Store markers.

```cpp
mat[i][0]=0;
mat[0][j]=0;
```

For the first column,

```cpp
if(mat[i][0]==0)
    col0=0;
```

---

### Step 2 — Update Inner Matrix

Ignore the first row and first column.

Traverse from

```
Bottom Right

↓

Top Left
```

If

```cpp
mat[i][0]==0 || mat[0][j]==0
```

make

```cpp
mat[i][j]=0;
```

---

### Step 3 — Update First Column

If

```cpp
col0==0
```

make the first column zero.

---

## Why Traverse Backwards?

The first row stores all the **column markers**.

If we update it too early,

those markers get destroyed before the remaining cells use them.

Therefore,

always traverse

```
Bottom Right

↓

Top Left
```

---

## DRY RUN

Input

```
1 1 1
1 0 1
1 1 1
```

---

### First Pass

Zero found at

```
(1,1)
```

Mark

```
mat[1][0]=0

mat[0][1]=0
```

Matrix

```
1 0 1
0 0 1
1 1 1
```

---

### Second Pass

```
(2,2)

No marker

Keep 1
```

```
(2,1)

Column marker found

↓

0
```

```
(1,2)

Row marker found

↓

0
```

Matrix

```
1 0 1
0 0 0
1 0 1
```

---

### First Column

```
col0==1
```

Do nothing.

Final Answer

```
1 0 1
0 0 0
1 0 1
```

---

## IMPORTANT CODE SNIPPETS

### Mark Rows & Columns

```cpp
if(mat[i][j]==0){

    mat[i][0]=0;
    mat[0][j]=0;
}
```

---

### Handle First Column

```cpp
if(mat[i][0]==0)
    col0=0;
```

---

### Reverse Traversal

```cpp
for(int i=n-1;i>=0;i--){

    for(int j=m-1;j>=1;j--){

        if(mat[i][0]==0 || mat[0][j]==0)
            mat[i][j]=0;
    }

    if(col0==0)
        mat[i][0]=0;
}
```

---

## COMMON MISTAKES

### ❌ Updating immediately after finding a zero

Creates cascading zeros.

---

### ❌ Forgetting that only original zeros matter

Always remember,

newly created zeros should **not** affect future decisions.

---

### ❌ Writing

```cpp
mat[0][1]=0;
```

instead of

```cpp
mat[0][j]=0;
```

Very common interview mistake.

---

### ❌ Forgetting `col0`

`mat[0][0]` cannot represent both

- first row
- first column

A separate variable is mandatory.

---

### ❌ Traversing from top-left during the second pass

Destroys the markers stored in the first row.

Always traverse from **bottom-right**.

---

## WHY I MIGHT FORGET THIS

There are **three special cases**.

- First Row
- First Column
- mat[0][0]

Remember

> **First Column stores Row Markers.**
>
> **First Row stores Column Markers.**
>
> **col0 handles the First Column separately.**

---

## INTERVIEW FLOW

1. Explain why immediate modification is wrong.
2. Present Method 1 using `vector<pair<int,int>>`.
3. Improve to Method 2 using `row[]` and `col[]`.
4. Observe that those arrays only store markers.
5. Replace them with the first row and first column.
6. Explain why `mat[0][0]` is ambiguous.
7. Introduce `col0`.
8. Explain why reverse traversal is necessary.
9. Conclude with `O(nm)` time and `O(1)` extra space.

---

## TIME COMPLEXITY

### Method 1

```
Scanning Matrix

O(nm)

Updating

O(k(n+m))

Worst Case

O(nm(n+m))
```

---

### Method 2

```
First Traversal

O(nm)

Second Traversal

O(nm)

Total

O(nm)
```

---

### Method 3

```
Marking

O(nm)

Reverse Traversal

O(nm)

Total

O(nm)
```

---

## SPACE COMPLEXITY

### Method 1

```
O(k)

Worst Case

O(nm)
```

---

### Method 2

```
row[]

O(n)

col[]

O(m)

Total

O(n+m)
```

---

### Method 3

Only one variable

```
col0
```

Extra Space

```
O(1)
```

---

## EDGE CASES

- Single Row Matrix
- Single Column Matrix
- Matrix with no zeros
- Matrix with all zeros
- Zero at `(0,0)`
- Zeros only in the first row
- Zeros only in the first column
- Multiple zeros in the same row
- Multiple zeros in the same column

---

## PATTERN RECOGNITION

Think of this pattern whenever you see

- Modify matrix based on existing cells.
- Original state must be preserved.
- Changes should not affect future decisions.
- Marker arrays can potentially be stored inside the matrix.

### Trigger Sentence

> **Mark First → Modify Later.**
>
> **If marker arrays only store flags, reuse the matrix itself.**

---

# Clean C++ Code (Optimal)

```cpp
class Solution {
public:
    void setMatrixZeroes(vector<vector<int>> &mat) {

        int n = mat.size();
        int m = mat[0].size();

        int col0 = 1;

        // Step 1 : Mark rows and columns
        for(int i=0;i<n;i++){

            if(mat[i][0]==0)
                col0=0;

            for(int j=1;j<m;j++){

                if(mat[i][j]==0){

                    mat[i][0]=0;
                    mat[0][j]=0;
                }
            }
        }

        // Step 2 : Update matrix from bottom-right
        for(int i=n-1;i>=0;i--){

            for(int j=m-1;j>=1;j--){

                if(mat[i][0]==0 || mat[0][j]==0)
                    mat[i][j]=0;
            }

            if(col0==0)
                mat[i][0]=0;
        }
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int col0 = 1;
```

Stores whether the **first column** should become zero because `mat[0][0]` is already used for the first row.

---

```cpp
if(mat[i][0]==0)
    col0=0;
```

Checks if the current row originally contains a zero in the first column.

---

```cpp
mat[i][0]=0;
mat[0][j]=0;
```

Uses the first column to mark rows and the first row to mark columns.

---

```cpp
for(int i=n-1;i>=0;i--)
```

Processes from bottom to top so the markers in the first row remain intact until all cells have used them.

---

```cpp
if(mat[i][0]==0 || mat[0][j]==0)
```

If either the row marker or the column marker is zero, the current cell must become zero.

---

```cpp
if(col0==0)
    mat[i][0]=0;
```

Updates the first column only after the remaining matrix has been processed.

---

# Easy-to-Remember Summary

### Method 1

Store every zero position.

```
vector<pair<int,int>>
```

Time

```
O(nm(n+m))
```

Space

```
O(nm)
```

---

### Method 2

Store only row and column flags.

```
row[]

col[]
```

Time

```
O(nm)
```

Space

```
O(n+m)
```

---

### Method 3

Reuse

- First Row → Column Markers
- First Column → Row Markers
- `col0` → First Column flag

Time

```
O(nm)
```

Space

```
O(1)
```

---

## One-Line Interview Memory Trick

> **Store markers first, modify later. First Column stores Row Markers, First Row stores Column Markers, and `col0` handles the First Column because `mat[0][0]` can't represent both.**
