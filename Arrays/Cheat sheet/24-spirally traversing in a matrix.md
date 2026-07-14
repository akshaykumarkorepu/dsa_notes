

## Question: Spirally Traversing a Matrix

### PATTERN: Boundary Traversal (Layer-by-Layer Traversal)
→ **Trigger:** "When I see a matrix that needs to be traversed layer-by-layer or in spiral order."

---

### OPTIMAL

→ **First instinct:**  
"I immediately maintain four boundaries (`top`, `bottom`, `left`, `right`) and keep shrinking the current rectangle after traversing its four sides."

→ **Core idea:**  
Maintain four boundary variables: `top`, `bottom`, `left`, and `right`, representing the current unvisited rectangle. Traverse **Top → Right → Bottom → Left** in every iteration, then update (`top++`, `right--`, `bottom--`, `left++`). Before traversing the bottom row and left column, check if those boundaries still exist to avoid duplicate traversal.

**Crucial Snippets**

```cpp
while(top <= bottom && left <= right)
```

```cpp
// Top
for(int j = left; j <= right; j++)
top++;
```

```cpp
// Right
for(int i = top; i <= bottom; i++)
right--;
```

```cpp
// Bottom
if(top <= bottom){
    for(int j = right; j >= left; j--)
    bottom--;
}
```

```cpp
// Left
if(left <= right){
    for(int i = bottom; i >= top; i--)
    left++;
}
```

→ **TC / SC:**  
**Time:** O(n × m)  
**Space:** O(1) Auxiliary Space

---

### WATCH OUT FOR

→ Forgetting the boundary checks:

```cpp
if(top <= bottom)
if(left <= right)
```

This causes duplicate traversal for single-row or single-column matrices.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. I'll treat the matrix as a shrinking rectangle.
2. I'll maintain four boundaries: `top`, `bottom`, `left`, and `right`.
3. Traverse in clockwise order: **Top → Right → Bottom → Left**.
4. After each traversal, shrink the corresponding boundary.
5. Use boundary checks before the bottom row and left column to avoid duplicate traversal.
````
