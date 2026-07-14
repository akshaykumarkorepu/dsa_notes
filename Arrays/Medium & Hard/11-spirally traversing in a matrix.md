
## PROBLEM:
Given an `n × m` matrix, return all elements in **clockwise spiral order**.

Example:

```text
1   2   3   4
5   6   7   8
9  10  11  12
13 14  15 16

Output:
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
```

---

## PATTERN:
**Boundary Traversal (Layer-by-Layer Traversal)**

---

## WHY THIS PATTERN:

A spiral traversal is nothing more than repeatedly traversing the **boundary of the current rectangle**.

Instead of remembering visited cells, we maintain **4 boundaries**:

- `top`
- `bottom`
- `left`
- `right`

These boundaries represent the current **unvisited rectangle**.

After completing one boundary traversal, we shrink the rectangle by updating the corresponding boundary.

---

## CORE IDEA:

Think of peeling an onion.

Initially,

```text
┌──────────────┐
│              │
│              │
│              │
└──────────────┘
```

Traverse its boundary:

```text
Top Row
↓

Right Column
↓

Bottom Row
↓

Left Column
```

After one complete traversal,

shrink the rectangle.

Repeat until no rectangle remains.

---

## BRUTE FORCE:

### Is brute force required?

**No.**

Although a solution using

- visited matrix
- direction vectors

exists, it

- uses extra space
- is harder to implement
- does not naturally lead to the optimal approach

Interviewers generally expect the boundary traversal directly.

---

## OPTIMAL APPROACH:

Maintain four boundaries:

```cpp
top = 0;
bottom = n-1;

left = 0;
right = m-1;
```

These define the current unvisited rectangle.

While the rectangle still exists,

traverse:

1. Top Row
2. Right Column
3. Bottom Row
4. Left Column

After every traversal,

shrink the corresponding boundary.

---

## ALGORITHM:

### Step 1

Initialize

```cpp
top = 0;
bottom = n-1;

left = 0;
right = m-1;
```

### Step 2

Traverse Top Row

```text
Left → Right
```

Then

```cpp
top++;
```

because the first row has been visited.

### Step 3

Traverse Right Column

```text
Top → Bottom
```

Then

```cpp
right--;
```

because the last column has been visited.

### Step 4

If

```cpp
top <= bottom
```

then traverse Bottom Row

```text
Right → Left
```

Then

```cpp
bottom--;
```

### Step 5

If

```cpp
left <= right
```

then traverse Left Column

```text
Bottom → Top
```

Then

```cpp
left++;
```

Repeat until

```cpp
top > bottom
```

or

```cpp
left > right
```

---

## DRY RUN:

Matrix

```text
1   2   3   4
5   6   7   8
9  10  11 12
13 14 15 16
```

Initially

```text
top = 0
bottom = 3

left = 0
right = 3
```

Current rectangle

```text
1   2   3   4
5   6   7   8
9  10  11 12
13 14 15 16
```

### Step 1 : Traverse Top Row

Loop

```cpp
for(int j = left; j <= right; j++)
```

Print

```text
1 2 3 4
```

Move

```cpp
top++;
```

Now

```text
top = 1
```

Remaining rectangle

```text
5   6   7   8
9  10  11 12
13 14 15 16
```

---

### Step 2 : Traverse Right Column

Loop

```cpp
for(int i = top; i <= bottom; i++)
```

Print

```text
8
12
16
```

Answer

```text
1 2 3 4 8 12 16
```

Move

```cpp
right--;
```

Now

```text
right = 2
```

Remaining rectangle

```text
5   6   7
9  10 11
13 14 15
```

---

### Step 3 : Traverse Bottom Row

Loop

```cpp
for(int j = right; j >= left; j--)
```

Print

```text
15
14
13
```

Answer

```text
1 2 3 4 8 12 16 15 14 13
```

Move

```cpp
bottom--;
```

Now

```text
bottom = 2
```

Remaining rectangle

```text
5 6 7
9 10 11
```

---

### Step 4 : Traverse Left Column

Loop

```cpp
for(int i = bottom; i >= top; i--)
```

Print

```text
9
5
```

Answer

```text
1 2 3 4 8 12 16 15 14 13 9 5
```

Move

```cpp
left++;
```

Now

```text
left = 1
```

Remaining rectangle

```text
6 7
10 11
```

Repeat the same steps.

Top

```text
6 7
```

Right

```text
11
```

Bottom

```text
10
```

Loop ends.

Final answer

```text
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
```

---

## IMPORTANT CODE SNIPPETS:

### Boundary Initialization

```cpp
int top = 0;
int bottom = n - 1;
int left = 0;
int right = m - 1;
```

### While Loop

```cpp
while(top <= bottom && left <= right)
```

Continue while a valid rectangle exists.

### Top Row

```cpp
for(int j = left; j <= right; j++)
    ans.push_back(mat[top][j]);

top++;
```

### Right Column

```cpp
for(int i = top; i <= bottom; i++)
    ans.push_back(mat[i][right]);

right--;
```

### Bottom Row

```cpp
if(top <= bottom){
    for(int j = right; j >= left; j--)
        ans.push_back(mat[bottom][j]);

    bottom--;
}
```

### Left Column

```cpp
if(left <= right){
    for(int i = bottom; i >= top; i--)
        ans.push_back(mat[i][left]);

    left++;
}
```

---

## COMMON MISTAKES:

❌ Forgetting

```cpp
if(top <= bottom)
```

Single-row matrices print the last row twice.

❌ Forgetting

```cpp
if(left <= right)
```

Single-column matrices print the same column twice.

❌ Traversing Bottom Left → Right

Wrong

```text
13 14 15
```

Correct

```text
15 14 13
```

because spiral must continue clockwise.

❌ Traversing Left Top → Bottom

Wrong

```text
5
9
```

Correct

```text
9
5
```

❌ Updating boundaries before traversal.

Always

```text
Traverse

↓

Update Boundary
```

❌ Forgetting

```cpp
return ans;
```

---

## WHY I MIGHT FORGET THIS:

Because I try to memorize **4 different loops**.

Instead remember only one idea:

> **Walk around the current rectangle.**

Then ask:

1. Which edge am I printing?
2. Which boundary becomes useless after printing it?

Everything else follows automatically.

---

## INTERVIEW FLOW:

> Since the output is spiral, I'll process the matrix layer by layer.

> I'll maintain four boundaries—top, bottom, left, and right—which represent the current unvisited rectangle.

> In every iteration, I'll traverse the four boundaries in clockwise order:

```text
Top Row
↓

Right Column
↓

Bottom Row
↓

Left Column
```

> After traversing each side, I'll shrink that boundary because it has already been visited.

> Before traversing the bottom row and left column, I'll check if they still exist to avoid duplicate traversal in single-row or single-column cases.

> Each element is visited exactly once.

---

## TIME COMPLEXITY:

**O(n × m)**

### Reason

Every element is visited exactly once.

No element is revisited.

---

## SPACE COMPLEXITY:

**O(1)** Auxiliary Space

Only four integer variables are used.

(The output vector is not counted as auxiliary space.)

---

## EDGE CASES:

### Single Row

```text
1 2 3
```

Need

```cpp
if(top <= bottom)
```

### Single Column

```text
1
2
3
```

Need

```cpp
if(left <= right)
```

### One Cell

```text
5
```

Output

```text
5
```

### Rectangular Matrix

```text
2 × 5
5 × 2
```

Algorithm remains unchanged.

### Empty Remaining Rectangle

Handled automatically by

```cpp
while(top <= bottom && left <= right)
```

---

## PATTERN RECOGNITION:

Whenever you hear:

- Spiral Matrix
- Spiral Traversal
- Print Matrix in Spiral Order
- Traverse Layer by Layer
- Matrix Rings
- Boundary Traversal

Think:

> **Current Rectangle + Four Boundaries**

Remember:

```text
Top
↓

Right
↓

Bottom
↓

Left
```

After every traversal

```text
top++

right--

bottom--

left++
```

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<int> spirallyTraverse(vector<vector<int>> &mat) {

        int n = mat.size();
        int m = mat[0].size();

        vector<int> ans;

        int top = 0;
        int bottom = n - 1;
        int left = 0;
        int right = m - 1;

        while (top <= bottom && left <= right) {

            // Traverse Top Row
            for (int j = left; j <= right; j++) {
                ans.push_back(mat[top][j]);
            }
            top++;

            // Traverse Right Column
            for (int i = top; i <= bottom; i++) {
                ans.push_back(mat[i][right]);
            }
            right--;

            // Traverse Bottom Row
            if (top <= bottom) {
                for (int j = right; j >= left; j--) {
                    ans.push_back(mat[bottom][j]);
                }
                bottom--;
            }

            // Traverse Left Column
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    ans.push_back(mat[i][left]);
                }
                left++;
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int top = 0;
int bottom = n - 1;
int left = 0;
int right = m - 1;
```

➡️ Defines the current unvisited rectangle.

```cpp
while(top <= bottom && left <= right)
```

➡️ Continue while a valid rectangle exists.

```cpp
mat[top][j]
```

➡️ Top row is fixed; only columns change.

```cpp
top++;
```

➡️ Top row has been visited.

```cpp
mat[i][right]
```

➡️ Right column is fixed; only rows change.

```cpp
right--;
```

➡️ Right column has been visited.

```cpp
if(top <= bottom)
```

➡️ Traverse the bottom row only if it still exists.

```cpp
mat[bottom][j]
```

➡️ Bottom row is fixed; traverse from **right to left** to maintain the clockwise spiral.

```cpp
bottom--;
```

➡️ Bottom row has been visited.

```cpp
if(left <= right)
```

➡️ Traverse the left column only if it still exists.

```cpp
mat[i][left]
```

➡️ Left column is fixed; traverse from **bottom to top** to complete the spiral.

```cpp
left++;
```

➡️ Left column has been visited.

---

# Easy-to-Remember Summary

### One Mental Picture

**Imagine walking around the boundary of a shrinking rectangle.**

### Traversal Order

```text
Top
↓

Right
↓

Bottom
↓

Left
```

### Boundary Updates

```text
Top Row     → top++

Right Col   → right--

Bottom Row  → bottom--

Left Col    → left++
```

### Two Golden Rules

- Print first, then shrink the boundary.
- Check `top <= bottom` and `left <= right` before printing the bottom row and left column.

### 10-Second Interview Memory Trick

> **TRBL + Shrink**

- **T**op → `top++`
- **R**ight → `right--`
- **B**ottom → `bottom--`
- **L**eft → `left++`

If you remember this, you can derive the entire solution during an interview instead of memorizing it.
