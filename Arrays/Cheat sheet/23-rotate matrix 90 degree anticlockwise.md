
# PATTERN: Matrix Transformation (Transpose + Reverse)
→ **Trigger:** "When I see an N×N matrix rotation **without extra space**."

---

# BRUTE FORCE
→ **Idea:** Create a temporary matrix and place every element at its rotated position using the mapping `(i, j) → (n-1-j, i)`, then copy it back.
→ **TC / SC:** **O(N²) / O(N²)**

---

# OPTIMAL

→ **First instinct:** "I immediately think of decomposing the rotation into **Transpose + Reverse Every Column**."

→ **Core idea:** First transpose the matrix by swapping only the upper triangle (`j = i + 1`) with its symmetric element across the diagonal. Then, for each column, use two pointers (`top`, `bottom`) to reverse it in-place. These two operations together produce a 90° anti-clockwise rotation using constant extra space.

**Crucial Snippets**

```cpp
// Transpose
for(int i=0;i<n;i++)
    for(int j=i+1;j<n;j++)
        swap(mat[i][j], mat[j][i]);
```

```cpp
// Reverse every column
for(int col=0; col<n; col++){
    int top=0, bottom=n-1;
    while(top<bottom){
        swap(mat[top][col], mat[bottom][col]);
        top++;
        bottom--;
    }
}
```

→ **TC / SC:** **O(N²) / O(1)**

---

# WATCH OUT FOR

→ Starting transpose with `j = 0` instead of `j = i + 1` swaps every pair twice and restores the original matrix.

---

# INTERVIEW FLOW

1. Matrix is square and rotation must be in-place.
2. Brute force uses an extra matrix with index mapping `(i,j) → (n-1-j,i)`.
3. Optimal is **Transpose + Reverse Every Column**.
4. During transpose, start `j` from `i+1` to avoid double swapping.
5. Reverse each column using two pointers (`top`, `bottom`) for **O(N²)** time and **O(1)** space.
````
