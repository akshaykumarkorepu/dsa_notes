

**Question:** Search in a Row & Column Sorted Matrix

---

## PATTERN: Staircase Search (Sorted Matrix Search)

→ **Trigger:** *"When I see a matrix where every row and every column is sorted, and I need to search for a single element."*

---

## BRUTE FORCE

→ **Idea:** Traverse every element in the matrix and compare it with the target.

→ **TC / SC:** `O(n × m)` / `O(1)`

---

## OPTIMAL

→ **First instinct:** *"I immediately start from the top-right corner and eliminate one entire row or one entire column after every comparison."*

→ **Core idea:** Maintain two pointers: `row = 0` and `col = m - 1`. At every step, compare `arr[row][col]` with the target. If it is larger, move left (`col--`) because everything below is even larger. If it is smaller, move down (`row++`) because everything to the left is even smaller. Continue until the target is found or the pointers move outside the matrix.

**Crucial Snippets**

```cpp
int row = 0;
int col = m - 1;
```

```cpp
while(row < n && col >= 0)
```

```cpp
if(arr[row][col] == x)
    return true;
else if(arr[row][col] > x)
    col--;
else
    row++;
```

→ **TC / SC:** `O(n + m)` / `O(1)`

---

## WATCH OUT FOR

→ Starting from the wrong corner (like top-left or bottom-right). Only the **top-right** (or equivalently bottom-left) lets you eliminate an entire row or column after every comparison.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every element in `O(n × m)`.
2. Since both rows and columns are sorted, I can eliminate an entire row or column after each comparison.
3. I start from the top-right corner because left is smaller and down is larger.
4. If current > target, move left; if current < target, move down.
5. The row moves at most `n` times and the column at most `m` times, giving `O(n + m)` time and `O(1)` space.
````
