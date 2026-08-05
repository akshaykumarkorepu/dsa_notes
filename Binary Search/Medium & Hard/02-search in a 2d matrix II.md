# Search in a Row & Column Sorted Matrix (Staircase Search)

## PROBLEM:

You are given a matrix where:

- Every **row is sorted** from left to right.
- Every **column is sorted** from top to bottom.

Return `true` if the target `x` exists in the matrix; otherwise return `false`.

---

## PATTERN:

**Sorted Matrix Search (Staircase Search)**

**Trigger:**

> "Every row is sorted and every column is sorted."

---

## WHY THIS PATTERN:

This matrix is **not globally sorted**, so we **cannot** flatten it into a 1D array and apply binary search.

Instead, we use both sorting properties together.

The **top-right corner** is special because:

- Everything **to the left is smaller**
- Everything **below is larger**

Therefore, after comparing one element, we can eliminate **an entire row or an entire column**.

---

## CORE IDEA:

Start from the **top-right corner**.

At every step:

- If current element == target → Found.
- If current element > target → Move Left.
- If current element < target → Move Down.

Each move removes one complete row or one complete column from consideration.

---

## BRUTE FORCE:

### Idea

Check every element one by one.

### Code

```cpp
class Solution {
public:
    bool matSearch(vector<vector<int>> &arr, int x) {

        int n = arr.size();
        int m = arr[0].size();

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {

                if(arr[i][j] == x)
                    return true;
            }
        }

        return false;
    }
};
```

### Dry Run

Matrix

```text
1 4 7
2 5 8
3 6 9

Target = 6
```

Comparisons

```text
1 ❌
4 ❌
7 ❌
2 ❌
5 ❌
8 ❌
3 ❌
6 ✅
```

Almost every element is checked.

### Time Complexity

```text
O(n × m)
```

### Space Complexity

```text
O(1)
```

### Why Optimize?

Although the matrix is sorted, brute force completely ignores that information.

We should use the sorting to eliminate unnecessary searches.

---

## OPTIMAL APPROACH:

Start from the **top-right corner**.

Suppose the current element is:

```text
25
```

### Case 1

```text
25 > Target
```

Everything below is even larger.

Therefore, the target cannot exist in this column.

Move Left.

---

### Case 2

```text
25 < Target
```

Everything left is even smaller.

Therefore, the target cannot exist in this row.

Move Down.

---

Each comparison removes an entire row or column.

---

## ALGORITHM:

### Step 1

Start from the top-right corner.

```cpp
row = 0;
col = m - 1;
```

---

### Step 2

Continue while inside the matrix.

```cpp
while(row < n && col >= 0)
```

---

### Step 3

If current element equals target,

```cpp
return true;
```

---

### Step 4

If current element is larger than target,

```cpp
col--;
```

Move Left.

---

### Step 5

Otherwise,

```cpp
row++;
```

Move Down.

---

### Step 6

If the loop finishes,

```cpp
return false;
```

The target does not exist.

---

## DRY RUN:

Matrix

```text
      0   1   2   3
    ----------------
0 |   10  20  30  40
1 |   15  25  35  45
2 |   27  29  37  48
3 |   32  33  39  50
```

Target

```text
29
```

---

### Initial Position

```text
row = 0
col = 3

Current = 40
```

Since

```text
40 > 29
```

Move Left.

---

### Now

```text
Current = 30
```

Since

```text
30 > 29
```

Move Left.

---

### Now

```text
Current = 20
```

Since

```text
20 < 29
```

Move Down.

---

### Now

```text
Current = 25
```

Since

```text
25 < 29
```

Move Down.

---

### Now

```text
Current = 29
```

Found.

Return `true`.

---

### Path Taken

```text
40
←
30
↓
20
↓
25
↓
29
```

Only **5 comparisons** instead of checking all **16 elements**.

---

## IMPORTANT CODE SNIPPETS:

### Start Position

```cpp
int row = 0;
int col = m - 1;
```

---

### Loop Condition

```cpp
while(row < n && col >= 0)
```

---

### Found

```cpp
if(arr[row][col] == x)
    return true;
```

---

### Move Left

```cpp
col--;
```

---

### Move Down

```cpp
row++;
```

---

## COMMON MISTAKES:

### 1. Starting from the Top-Left

Top-left cannot uniquely determine where to move.

---

### 2. Starting from the Bottom-Right

Same problem.

---

### 3. Wrong Loop Condition

Incorrect

```cpp
while(row <= n)
```

Correct

```cpp
while(row < n && col >= 0)
```

---

### 4. Moving in the Wrong Direction

Remember

```text
Current > Target
Move Left

Current < Target
Move Down
```

---

### 5. Using

```cpp
col++;
```

instead of

```cpp
col--;
```

---

## WHY I MIGHT FORGET THIS:

Remember this picture.

```text
Top Right

        Smaller ← Current
                   |
                   |
                 Larger
```

If the current value is too large,

go toward the **smaller side (Left).**

If the current value is too small,

go toward the **larger side (Down).**

---

## INTERVIEW FLOW:

> "The brute force solution checks every element and takes O(n × m), but it ignores the sorting properties of the matrix.

> Since every row and every column is sorted, I start from the top-right corner.

> If the current element is larger than the target, everything below it is even larger, so I eliminate the entire column by moving left.

> If the current element is smaller than the target, everything to the left is smaller, so I eliminate the entire row by moving down.

> Each comparison removes one row or one column, giving an O(n + m) solution."

---

## TIME COMPLEXITY:

### Time Complexity: **O(n + m)**

Where:

- `n` = number of rows
- `m` = number of columns

### Why?

We start from the top-right corner.

At every iteration, exactly **one** of these happens:

```cpp
col--;
```

or

```cpp
row++;
```

Notice:

- `row` never decreases.
- `col` never increases.

Therefore,

- `row` can move down at most **n** times.
- `col` can move left at most **m** times.

Maximum iterations

```text
n + m
```

Hence,

```text
Time Complexity = O(n + m)
```

---

## SPACE COMPLEXITY:

We only store:

```cpp
int n;
int m;
int row;
int col;
```

No additional data structures are created.

Therefore,

```text
Space Complexity = O(1)
```

---

## EDGE CASES:

### Single Element

```text
5
```

Target = 5

Return `true`.

---

### Single Row

```text
1 3 5 7
```

Works correctly.

---

### Single Column

```text
1
3
5
7
```

Works correctly.

---

### Target Smaller Than the Smallest Element

```text
Target = 0
```

Eventually moves left outside the matrix.

---

### Target Larger Than the Largest Element

```text
Target = 100
```

Eventually moves down outside the matrix.

---

### Target Not Present

Loop finishes.

Return `false`.

---

## PATTERN RECOGNITION:

Use **Staircase Search** whenever you see:

- ✅ Every row is sorted.
- ✅ Every column is sorted.
- ✅ Need to search for a single element.

Immediately think:

```text
Start at Top-Right

Current == Target → Found
Current > Target  → Move Left
Current < Target  → Move Down
```

**Memory Trick**

```text
Top-Right

← Smaller
↓ Larger
```

If the matrix is instead **globally sorted** (the first element of each row is greater than the last element of the previous row), then use **Binary Search on a Flattened Matrix** instead.

---

# Clean C++ Code

```cpp
class Solution {
public:
    bool matSearch(vector<vector<int>> &arr, int x) {

        int n = arr.size();
        int m = arr[0].size();

        int row = 0;
        int col = m - 1;

        while (row < n && col >= 0) {

            if (arr[row][col] == x) {
                return true;
            }
            else if (arr[row][col] > x) {
                col--;
            }
            else {
                row++;
            }
        }

        return false;
    }
};
```

---

# Intuition Behind Every Important Line

### Get Matrix Dimensions

```cpp
int n = arr.size();
int m = arr[0].size();
```

We need the boundaries of the matrix.

---

### Start at the Top-Right Corner

```cpp
int row = 0;
int col = m - 1;
```

This corner uniquely gives:

- Left → Smaller
- Down → Larger

---

### Stay Inside the Matrix

```cpp
while (row < n && col >= 0)
```

Stop once we move outside the matrix.

---

### Found the Target

```cpp
if (arr[row][col] == x)
    return true;
```

No further searching is required.

---

### Current Value is Too Large

```cpp
else if (arr[row][col] > x)
    col--;
```

Everything below is larger.

Discard the entire column.

---

### Current Value is Too Small

```cpp
else
    row++;
```

Everything left is smaller.

Discard the entire row.

---

### Target Not Found

```cpp
return false;
```

Every possible candidate has already been eliminated.

---

# Easy-to-Remember Summary

```text
PATTERN:
Row + Column Sorted Matrix → Staircase Search

START:
Top-Right Corner

RULE:
Current == Target → Found
Current > Target  → Move Left
Current < Target  → Move Down

WHY?
Left = Smaller
Down = Larger

TIME:
O(n + m)

SPACE:
O(1)

MEMORY TRICK:
Top-Right
← Smaller
↓ Larger
```
