# NOTE

## PROBLEM:

Given a **strictly sorted 2D matrix**, determine whether a target `x` exists.

A matrix is **strictly sorted** if:
- Every row is sorted in increasing order.
- The first element of every row is greater than the last element of the previous row.

Example:

```
1   5   9
14 20 21
30 34 43
```

Expected Complexity:
- **Time:** O(log(n × m))
- **Space:** O(1)

---

## PATTERN:

**Binary Search on a Virtually Flattened Sorted Array**

---

## WHY THIS PATTERN:

Although the input is a **2D matrix**, it behaves exactly like **one sorted 1D array** because:

- Each row is sorted.
- Every row starts with a value greater than the previous row's last value.

Example:

```
Matrix:

1   5   9
14 20 21
30 34 43

Behaves as:

1 5 9 14 20 21 30 34 43
```

Since the data is globally sorted, **Binary Search** is the optimal choice.

---

## CORE IDEA:

Instead of physically flattening the matrix:

- Imagine it as one sorted array.
- Perform Binary Search on indices from `0` to `n*m - 1`.
- Convert the 1D index back to matrix coordinates whenever needed.

Conversion:

```cpp
row = index / m;
col = index % m;
```

---

## BRUTE FORCE:

### Intuition

Check every element one by one.

If found → return `true`.

Otherwise → return `false`.

### Code

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& mat, int x) {

        int n = mat.size();
        int m = mat[0].size();

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(mat[i][j] == x)
                    return true;
            }
        }

        return false;
    }
};
```

### Time Complexity

```
O(n × m)
```

### Space Complexity

```
O(1)
```

### Why Optimize?

Brute force ignores the fact that the matrix is sorted.

Whenever data is sorted, Binary Search should be considered.

---

## OPTIMAL APPROACH:

Treat the matrix as a **virtual sorted array**.

Perform Binary Search on indices instead of actual matrix positions.

Convert every middle index into `(row, col)` using:

```cpp
row = mid / m;
col = mid % m;
```

---

## ALGORITHM:

1. Store the number of rows (`n`) and columns (`m`).
2. Set:
   - `low = 0`
   - `high = n*m - 1`
3. While `low <= high`:
   - Compute `mid`.
   - Convert `mid` into `(row, col)`.
   - Compare `mat[row][col]` with `x`.
   - If equal → return `true`.
   - If larger → search left half.
   - Otherwise → search right half.
4. If loop ends, return `false`.

---

## DRY RUN:

Matrix

```
1   5   9
14 20 21
30 34 43
```

Target = **30**

### Initial

```
low = 0
high = 8
```

### Iteration 1

```
mid = 4

row = 4 / 3 = 1
col = 4 % 3 = 1

Value = 20
```

20 < 30

```
low = 5
```

---

### Iteration 2

```
low = 5
high = 8

mid = 6

row = 6 / 3 = 2
col = 6 % 3 = 0

Value = 30
```

Target found.

Return `true`.

---

## IMPORTANT CODE SNIPPETS:

### Binary Search Range

```cpp
int low = 0;
int high = n * m - 1;
```

---

### Safe Mid

```cpp
int mid = low + (high - low) / 2;
```

---

### Convert Index → Row

```cpp
int row = mid / m;
```

---

### Convert Index → Column

```cpp
int col = mid % m;
```

---

## COMMON MISTAKES:

- Using `row = mid % m`.
- Using `col = mid / m`.
- Setting `high = n*m` instead of `n*m - 1`.
- Using `(low + high)/2` instead of the safe mid formula.
- Physically flattening the matrix (unnecessary).

---

## WHY I MIGHT FORGET THIS:

Most confusion comes from converting a 1D index back into matrix coordinates.

Remember:

```
Division → Row
Modulus  → Column
```

Example:

```
m = 4

Indices

0 1 2 3
4 5 6 7
8 9 10 11

Index = 9

row = 9 / 4 = 2
col = 9 % 4 = 1
```

So,

```
matrix[2][1]
```

---

## INTERVIEW FLOW:

> Since every row starts after the previous row ends, the entire matrix is globally sorted. Instead of treating it as a 2D structure, I imagine it as a single sorted array of size `n*m`. I perform Binary Search on indices from `0` to `n*m-1` and convert each middle index into `(row, col)` using `row = mid/m` and `col = mid%m`. This achieves `O(log(n*m))` time without extra space.

---

## TIME COMPLEXITY:

Binary Search halves the search space each iteration.

Total elements = `n × m`

Therefore,

```
O(log(n × m))
```

---

## SPACE COMPLEXITY:

Only a few variables are used.

```
O(1)
```

---

## EDGE CASES:

- Single element matrix.
- Single row matrix.
- Single column matrix.
- Target smaller than the first element.
- Target larger than the last element.
- Target not present.

---

## PATTERN RECOGNITION:

Think of this pattern whenever:

- Every row is sorted.
- First element of each row > last element of previous row.
- Expected complexity is `O(log(n*m))`.
- Need to search for one element.

Mental picture:

```
Matrix

1 5 9
14 20 21
30 34 43

↓

Virtual Array

1 5 9 14 20 21 30 34 43
```

No actual flattening is required.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>> &mat, int x) {

        int n = mat.size();
        int m = mat[0].size();

        int low = 0;
        int high = n * m - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int row = mid / m;
            int col = mid % m;

            if (mat[row][col] == x)
                return true;

            else if (mat[row][col] > x)
                high = mid - 1;

            else
                low = mid + 1;
        }

        return false;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int n = mat.size();
```
→ Number of rows.

```cpp
int m = mat[0].size();
```
→ Number of columns (needed for index conversion).

```cpp
int low = 0;
int high = n * m - 1;
```
→ Binary Search over the entire virtual array.

```cpp
int mid = low + (high - low) / 2;
```
→ Safe way to compute the middle index.

```cpp
int row = mid / m;
```
→ Finds which row the virtual index belongs to.

```cpp
int col = mid % m;
```
→ Finds the column within that row.

```cpp
if(mat[row][col] == x)
```
→ Target found.

```cpp
high = mid - 1;
```
→ Search left half.

```cpp
low = mid + 1;
```
→ Search right half.

```cpp
return false;
```
→ Target doesn't exist.

---

# EASY-TO-REMEMBER SUMMARY

- Matrix is **globally sorted**.
- Treat it as a **virtual sorted array**.
- Binary Search on indices.
- Convert index using:

```cpp
row = index / m;
col = index % m;
```

### Memory Trick

```
Flatten Mentally
↓

Binary Search Normally
↓

Division → Row
Modulus → Column
```
