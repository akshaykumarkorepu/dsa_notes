
**Question:** Search in a Sorted Matrix

---

## PATTERN: Binary Search on a Virtually Flattened Sorted Array
→ **Trigger:** "When I see a matrix where every row is sorted and the first element of each row is greater than the last element of the previous row."

---

## BRUTE FORCE
→ **Idea:** Traverse every element of the matrix and compare it with the target.
→ **TC / SC:** **O(n × m)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately treat the matrix as one sorted array and perform Binary Search."

→ **Core idea:** Perform Binary Search on indices from `0` to `n*m - 1` without actually flattening the matrix. For every `mid`, convert it into matrix coordinates using `row = mid / m` and `col = mid % m`, compare `mat[row][col]` with the target, and move the search space accordingly.

### Crucial Snippets

```cpp
int low = 0;
int high = n * m - 1;
```

```cpp
int mid = low + (high - low) / 2;
```

```cpp
int row = mid / m;
int col = mid % m;
```

```cpp
if(mat[row][col] == x)
    return true;
else if(mat[row][col] > x)
    high = mid - 1;
else
    low = mid + 1;
```

→ **TC / SC:** **O(log(n × m))** / **O(1)**

---

## WATCH OUT FOR

→ Using the wrong conversion formula. Always remember:

```cpp
row = mid / m;
col = mid % m;
```

---

## INTERVIEW FLOW

1. Matrix is globally sorted.
2. Treat it as one virtual sorted array.
3. Binary Search from `0` to `n*m - 1`.
4. Convert `mid` to `(row, col)` using `/` and `%`.
5. Compare and shrink the search space until found or exhausted.
