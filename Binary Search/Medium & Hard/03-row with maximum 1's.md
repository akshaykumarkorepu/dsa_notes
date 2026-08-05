# Row with Maximum 1s (Row-wise Sorted Matrix)

## PROBLEM:

You are given a binary matrix where:

- Every row is sorted in non-decreasing order (all `0`s come first, then all `1`s).
- Find the **index of the first row that contains the maximum number of 1s**.
- If no row contains any `1`, return `-1`.

---

## PATTERN:

**Binary Search on Every Row (First Occurrence Pattern)**

**Trigger:**

> "Each row is sorted and I need to count something (number of 1s)."

---

## WHY THIS PATTERN:

Since every row is sorted,

```
0 0 0 1 1 1
```

Instead of counting every element,

we only need to know

> **Where does the first 1 appear?**

Once we know its position,

```
Number of 1s = Total Columns - First One Index
```

Finding the first `1` is exactly a **First Occurrence Binary Search** problem.

So instead of spending **O(M)** per row,

we spend only

```
O(log M)
```

---

## CORE IDEA:

For every row:

1. Find the first `1` using Binary Search.
2. Compute

```
ones = m - firstOneIndex
```

3. Keep track of the row having maximum ones.

---

## BRUTE FORCE:

### Idea

Simply count every `1` in every row.

Example

```
Row 0 → 3 ones

Row 1 → 2 ones

Row 2 → 4 ones

Row 3 → 0 ones
```

Return the row having the largest count.

### Code

```cpp
int maxOnes = 0;
int ans = -1;

for(int i=0;i<n;i++){

    int count = 0;

    for(int j=0;j<m;j++){

        if(arr[i][j]==1)
            count++;
    }

    if(count > maxOnes){
        maxOnes = count;
        ans = i;
    }
}
```

### Time Complexity

```
O(N × M)
```

### Space Complexity

```
O(1)
```

---

### Transition to Optimal

Observe that each row is sorted.

```
0 0 0 1 1 1
```

Instead of counting every element,

we only need to find where the `1`s start.

That is exactly what Binary Search is good at.

---

## OPTIMAL APPROACH:

For every row:

```
Find First 1
        ↓
Count Ones
        ↓
Update Answer
```

Each Binary Search takes

```
O(log M)
```

Doing this for all rows gives

```
O(N log M)
```

---

## ALGORITHM:

### Step 1

Initialize

```
maxOnes = 0
ans = -1
```

---

### Step 2

For every row,

perform Binary Search to find the first occurrence of `1`.

---

### Step 3

Binary Search

```
low = 0
high = m-1
```

While

```
low <= high
```

Compute

```
mid
```

If

```
arr[mid] == 1
```

Store the answer and move left.

```
firstOne = mid
high = mid-1
```

Else

```
low = mid+1
```

---

### Step 4

Compute

```
ones = m - firstOne
```

If no `1` exists,

```
ones = 0
```

---

### Step 5

If

```
ones > maxOnes
```

Update

```
maxOnes
ans
```

Notice we use

```
>
```

instead of

```
>=
```

because we need the **first** row.

---

## DRY RUN:

Matrix

```
0 1 1 1

0 0 1 1

1 1 1 1

0 0 0 0
```

Rows = 4

Columns = 4

---

### Row 0

```
0 1 1 1
```

Binary Search

```
First 1 = Index 1
```

Ones

```
4 - 1 = 3
```

Current

```
maxOnes = 3
ans = 0
```

---

### Row 1

```
0 0 1 1
```

Binary Search

```
First 1 = Index 2
```

Ones

```
2
```

No update.

---

### Row 2

```
1 1 1 1
```

Binary Search

```
First 1 = Index 0
```

Ones

```
4
```

Update

```
maxOnes = 4
ans = 2
```

---

### Row 3

```
0 0 0 0
```

Binary Search

No `1` found.

```
ones = 0
```

No update.

Final Answer

```
2
```

---

## IMPORTANT CODE SNIPPETS:

### Binary Search

```cpp
int low = 0;
int high = m - 1;
int firstOne = -1;

while(low <= high){

    int mid = low + (high-low)/2;

    if(arr[i][mid] == 1){
        firstOne = mid;
        high = mid - 1;
    }
    else{
        low = mid + 1;
    }
}
```

---

### Count Ones

```cpp
int ones = (firstOne == -1) ? 0 : m - firstOne;
```

---

### Update Answer

```cpp
if(ones > maxOnes){
    maxOnes = ones;
    ans = i;
}
```

---

## COMMON MISTAKES:

### Mistake 1

Using

```cpp
>=
```

instead of

```cpp
>
```

This returns the **last** row instead of the **first** row.

---

### Mistake 2

Returning

```
firstOne
```

instead of

```
m - firstOne
```

Binary Search gives the position, not the count.

---

### Mistake 3

Not handling the case where no `1` exists.

Need

```
ones = 0
```

---

### Mistake 4

Searching for the last `1`.

We need the **first** `1`.

---

## WHY I MIGHT FORGET THIS:

The problem asks for

> "Maximum number of 1s"

So it's natural to think

> "Count every 1."

Instead think

> "Rows are sorted."

Then immediately ask

> "Can Binary Search find where 1s begin?"

That observation gives the optimization.

---

## INTERVIEW FLOW:

> Since every row is sorted, instead of counting every element, I Binary Search each row to find the first occurrence of `1`. Once I know its index, the number of `1`s is simply `columns - firstOneIndex`. I keep track of the row having the maximum count. Each Binary Search takes `O(log M)`, so the total complexity becomes `O(N log M)` with `O(1)` extra space.

---

## TIME COMPLEXITY:

Each row

```
Binary Search = O(log M)
```

There are

```
N rows
```

Overall

```
O(N log M)
```

Reason:

Every Binary Search cuts the search space in half.

---

## SPACE COMPLEXITY:

```
O(1)
```

Reason:

Only a few variables are used.

---

## EDGE CASES:

### 1. All zeros

```
0 0

0 0
```

Return

```
-1
```

---

### 2. All ones

```
1 1

1 1
```

Return

```
0
```

---

### 3. Single row

```
0 1 1
```

Return

```
0
```

---

### 4. Single column

```
0

1

1
```

Return

```
1
```

---

### 5. Tie

```
0 1 1

0 1 1
```

Return

```
0
```

because we use `>` instead of `>=`.

---

## PATTERN RECOGNITION:

Use this pattern whenever:

- Rows are sorted.
- Need the count of 1s/positives.
- Need the first occurrence.
- Need the last occurrence.
- Lower Bound / Upper Bound problems.

Typical Questions:

- Row with Maximum 1s
- Count Negatives in Sorted Row
- Count Positives
- First Occurrence
- Last Occurrence
- Lower Bound

---

# Clean C++ Code

```cpp
class Solution {
public:
    int rowWithMax1s(vector<vector<int>> &arr) {

        int n = arr.size();
        int m = arr[0].size();

        int maxOnes = 0;
        int ans = -1;

        for (int i = 0; i < n; i++) {

            int low = 0;
            int high = m - 1;
            int firstOne = -1;

            // Find the first occurrence of 1
            while (low <= high) {

                int mid = low + (high - low) / 2;

                if (arr[i][mid] == 1) {
                    firstOne = mid;
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }

            int ones = (firstOne == -1) ? 0 : m - firstOne;

            if (ones > maxOnes) {
                maxOnes = ones;
                ans = i;
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Get matrix dimensions

```cpp
int n = arr.size();
int m = arr[0].size();
```

We need the number of rows and columns.

---

### Store the best answer

```cpp
int maxOnes = 0;
int ans = -1;
```

- `maxOnes` stores the highest number of `1`s seen so far.
- `ans` stores its row index.
- If every row has zero `1`s, answer remains `-1`.

---

### Traverse every row

```cpp
for(int i = 0; i < n; i++)
```

Each row is processed independently.

---

### Binary Search variables

```cpp
int low = 0;
int high = m - 1;
int firstOne = -1;
```

- `low` and `high` define the search range.
- `firstOne` stores the leftmost `1` found.

---

### Continue Binary Search

```cpp
while(low <= high)
```

Search until the range becomes empty.

---

### Find middle safely

```cpp
int mid = low + (high-low)/2;
```

Prevents integer overflow.

---

### If current element is 1

```cpp
if(arr[i][mid] == 1)
```

A `1` is found, but it may not be the first one.

---

### Save answer and move left

```cpp
firstOne = mid;
high = mid - 1;
```

Store the current index and continue searching the left half.

---

### If current element is 0

```cpp
low = mid + 1;
```

Since rows are sorted, the first `1` must lie to the right.

---

### Count the number of ones

```cpp
int ones = (firstOne == -1) ? 0 : m - firstOne;
```

If no `1` exists,

```
ones = 0
```

Otherwise,

```
Total Columns - First One Index
```

---

### Update the answer

```cpp
if(ones > maxOnes)
```

Use **strictly greater (`>`)** so that in case of a tie, the first row remains the answer.

---

### Return answer

```cpp
return ans;
```

Returns the required row index or `-1`.

---

# Easy-to-Remember Summary

- **Sorted row → Don't count manually.**
- **Binary Search for the first `1`.**
- **Count = Columns − First One Index.**
- **Keep the row with maximum count.**
- **Use `>` instead of `>=` to preserve the first row.**

### Memory Trick

> **Sorted Row → First 1 → Count = m − firstOne → Keep Maximum**
