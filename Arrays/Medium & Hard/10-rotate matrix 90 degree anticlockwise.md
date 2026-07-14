
## PROBLEM:
Given an **N × N square matrix**, rotate it by **90° anti-clockwise** **without using any extra space**.

Example:

**Input**

```text
1 2 3
4 5 6
7 8 9
```

**Output**

```text
3 6 9
2 5 8
1 4 7
```

The challenge is that the rotation must happen **in-place**.

---

# PATTERN:

**Matrix Transformation**

Specifically:

- Matrix Transpose
- Matrix Reversal
- In-place Matrix Manipulation

---

# WHY THIS PATTERN:

Whenever a problem asks:

- Rotate Matrix
- Rotate Image
- Matrix Transformation
- Perform operation **without extra space**

Think:

> **Can I break the transformation into multiple simple in-place operations?**

For matrix rotation,

the answer is yes.

For

```text
90° Anti-clockwise
```

Use

```text
Transpose
+
Reverse Every Column
```

For

```text
90° Clockwise
```

Use

```text
Transpose
+
Reverse Every Row
```

---

# CORE IDEA:

Instead of moving every element individually,

transform the whole matrix using two operations.

### Step 1

Transpose the matrix.

```text
Rows become columns.
```

Original

```text
1 2 3
4 5 6
7 8 9
```

↓

Transpose

```text
1 4 7
2 5 8
3 6 9
```

---

### Step 2

Reverse every column.

Column 0

```text
1
2
3

↓

3
2
1
```

Column 1

```text
4
5
6

↓

6
5
4
```

Column 2

```text
7
8
9

↓

9
8
7
```

Final matrix

```text
3 6 9
2 5 8
1 4 7
```

No extra matrix is required.

---

# BRUTE FORCE:

## Intuition

The easiest way is to create another matrix.

Every element moves to its rotated position.

The mapping is

```text
(i,j)

↓

(n-1-j , i)
```

Store every element in its new position inside a temporary matrix.

Finally,

copy the temporary matrix back.

---

## Code

```cpp
class Solution {
public:
    void rotateMatrix(vector<vector<int>>& mat) {

        int n = mat.size();

        vector<vector<int>> temp(n, vector<int>(n));

        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < n; j++)
            {
                temp[n - 1 - j][i] = mat[i][j];
            }
        }

        mat = temp;
    }
};
```

---

## Dry Run

Input

```text
1 2 3
4 5 6
7 8 9
```

Initially

```text
0 0 0
0 0 0
0 0 0
```

Move

```text
1

(0,0)

↓

(2,0)
```

```text
0 0 0
0 0 0
1 0 0
```

---

Move

```text
2

↓

(1,0)
```

```text
0 0 0
2 0 0
1 0 0
```

---

Move

```text
3

↓

(0,0)
```

```text
3 0 0
2 0 0
1 0 0
```

Continue similarly.

Final matrix

```text
3 6 9
2 5 8
1 4 7
```

---

## Time Complexity

```text
Creating temp matrix → O(N²)

Copy back → O(N²)

Overall → O(N²)
```

---

## Space Complexity

```text
Extra Matrix

↓

O(N²)
```

---

# OPTIMAL APPROACH:

Instead of calculating the destination of every element,

perform two matrix transformations.

```text
Original

↓

Transpose

↓

Reverse Every Column

↓

Rotated Matrix
```

Everything happens inside the same matrix.

---

# ALGORITHM:

### Step 1

Transpose the matrix.

Swap

```text
mat[i][j]

↔

mat[j][i]
```

Only swap the upper triangle.

```cpp
for(int i = 0; i < n; i++)
{
    for(int j = i + 1; j < n; j++)
    {
        swap(mat[i][j], mat[j][i]);
    }
}
```

---

### Why does `j` start from `i+1`?

Suppose we swap

```text
2 ↔ 4
```

Later,

if we again reach

```text
4 ↔ 2
```

they return to their original positions.

So every pair gets swapped twice.

To avoid this,

start from

```text
j = i + 1
```

Every pair is swapped exactly once.

---

### Step 2

Reverse every column.

For every column,

place one pointer at the top

and another at the bottom.

Swap them.

Move inward.

Exactly like reversing an array.

```cpp
for(int col = 0; col < n; col++)
{
    int top = 0;
    int bottom = n - 1;

    while(top < bottom)
    {
        swap(mat[top][col], mat[bottom][col]);

        top++;
        bottom--;
    }
}
```

---

# DRY RUN:

Input

```text
1 2 3
4 5 6
7 8 9
```

---

## Step 1

Transpose

Swap

```text
2 ↔ 4
```

```text
1 4 3
2 5 6
7 8 9
```

---

Swap

```text
3 ↔ 7
```

```text
1 4 7
2 5 6
3 8 9
```

---

Swap

```text
6 ↔ 8
```

```text
1 4 7
2 5 8
3 6 9
```

Transpose complete.

---

## Step 2

Reverse Column 0

Before

```text
1
2
3
```

After

```text
3
2
1
```

Matrix

```text
3 4 7
2 5 8
1 6 9
```

---

Reverse Column 1

```text
4
5
6

↓

6
5
4
```

Matrix

```text
3 6 7
2 5 8
1 4 9
```

---

Reverse Column 2

```text
7
8
9

↓

9
8
7
```

Final

```text
3 6 9
2 5 8
1 4 7
```

Correct answer.

---

# IMPORTANT CODE SNIPPETS:

## Matrix Transpose

```cpp
for(int i = 0; i < n; i++)
{
    for(int j = i + 1; j < n; j++)
    {
        swap(mat[i][j], mat[j][i]);
    }
}
```

---

## Reverse Every Column

```cpp
for(int col = 0; col < n; col++)
{
    int top = 0;
    int bottom = n - 1;

    while(top < bottom)
    {
        swap(mat[top][col], mat[bottom][col]);
        top++;
        bottom--;
    }
}
```

---

# COMMON MISTAKES:

### Mistake 1

Using

```cpp
for(int j = 0; j < n; j++)
```

during transpose.

Every pair gets swapped twice.

---

### Mistake 2

Reversing rows instead of columns.

Rows produce

```text
90° Clockwise
```

Not anti-clockwise.

---

### Mistake 3

Using another matrix.

The problem explicitly asks

```text
Without extra space
```

---

### Mistake 4

Confusing clockwise and anti-clockwise.

Remember

```text
Clockwise

Transpose
+
Reverse Rows
```

```text
Anti-clockwise

Transpose
+
Reverse Columns
```

---

# WHY I MIGHT FORGET THIS:

Both rotations begin with

```text
Transpose
```

The only difference is

```text
Clockwise

↓

Reverse Rows
```

```text
Anti-clockwise

↓

Reverse Columns
```

Remember:

> **Rows → Clockwise**
>
> **Columns → Anti-clockwise**

---

# INTERVIEW FLOW:

> Since the problem doesn't allow extra space, using another matrix isn't acceptable.
>
> Instead of moving each element individually, I can decompose the rotation into two in-place transformations.
>
> First, I transpose the matrix by swapping elements across the main diagonal.
>
> Then, I reverse every column.
>
> These two operations together produce a 90° anti-clockwise rotation while using only constant extra space.

---

# TIME COMPLEXITY:

### Brute Force

Creating temporary matrix

```text
O(N²)
```

Copying back

```text
O(N²)
```

Overall

```text
O(N²)
```

---

### Optimal

Transpose

```text
≈ N(N−1)/2 swaps

↓

O(N²)
```

Reverse columns

```text
N columns

×

O(N)

↓

O(N²)
```

Overall

```text
O(N²)
```

---

# SPACE COMPLEXITY:

### Brute Force

Temporary matrix

```text
O(N²)
```

---

### Optimal

Only uses

```text
i
j
top
bottom
col
```

No extra matrix.

```text
O(1)
```

---

# EDGE CASES:

- Single element matrix
- 2×2 matrix
- All elements same
- Large matrix
- Negative numbers (works the same)
- Matrix with duplicate values

---

# PATTERN RECOGNITION:

Whenever you see:

- Rotate Matrix
- Rotate Image
- Matrix Transformation
- Rotate without extra space
- Flip Matrix

Immediately think:

```text
Can this transformation
be broken into

Transpose

+

One Reverse?
```

Remember:

| Rotation | Formula |
|----------|----------|
| 90° Clockwise | Transpose + Reverse Rows |
| 90° Anti-clockwise | Transpose + Reverse Columns |

---

# CLEAN C++ CODE (Optimal)

```cpp
class Solution {
public:
    void rotateMatrix(vector<vector<int>>& mat) {

        int n = mat.size();

        // Step 1: Transpose
        for(int i = 0; i < n; i++)
        {
            for(int j = i + 1; j < n; j++)
            {
                swap(mat[i][j], mat[j][i]);
            }
        }

        // Step 2: Reverse every column
        for(int col = 0; col < n; col++)
        {
            int top = 0;
            int bottom = n - 1;

            while(top < bottom)
            {
                swap(mat[top][col], mat[bottom][col]);
                top++;
                bottom--;
            }
        }
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int n = mat.size();
```

Stores the size of the matrix.

---

```cpp
for(int i = 0; i < n; i++)
```

Traverse every row.

---

```cpp
for(int j = i + 1; j < n; j++)
```

Start from `i + 1` so every pair is swapped exactly once.

---

```cpp
swap(mat[i][j], mat[j][i]);
```

Transpose the matrix by swapping symmetric elements.

---

```cpp
for(int col = 0; col < n; col++)
```

Process one column at a time.

---

```cpp
int top = 0;
int bottom = n - 1;
```

Initialize two pointers for reversing the column.

---

```cpp
while(top < bottom)
```

Keep swapping until both pointers meet.

---

```cpp
swap(mat[top][col], mat[bottom][col]);
```

Reverse the current column in-place.

---

```cpp
top++;
bottom--;
```

Move pointers inward.

---

# EASY-TO-REMEMBER SUMMARY

### Brute Force

```text
Find every element's new position

↓

Store in another matrix

↓

Copy back
```

Mapping:

```text
(i,j)

↓

(n-1-j , i)
```

Time: **O(N²)**

Space: **O(N²)**

---

### Optimal

```text
Transpose

↓

Reverse Every Column

↓

Done
```

Time: **O(N²)**

Space: **O(1)**

---

## ⭐ Memory Trick

```text
Clockwise

Transpose
+
Reverse Rows
```

```text
Anti-clockwise

Transpose
+
Reverse Columns
```

> **Transpose is always first. The direction is decided by what you reverse next.**
