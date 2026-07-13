

## Question: Set Matrix Zeroes

### PATTERN: Matrix Marking / In-place State Encoding
→ **Trigger:** *"When I need to modify a matrix based on its original values without letting new updates affect future decisions."*

---

## BRUTE FORCE (Method 1)
→ **Idea:** Store all original zero positions using `vector<pair<int,int>>`, then zero their entire rows and columns.
→ **Crucial Snippet:**
```cpp
vector<pair<int,int>> zeros;

if(mat[i][j]==0)
    zeros.push_back({i,j});
```
→ **TC / SC:** `O(nm + k(n+m))` (Worst: `O(nm(n+m)))` / `O(nm)`

---

## BETTER (Method 2)
→ **Idea:** Instead of storing every zero position, maintain two marker arrays (`row[]` and `col[]`) to remember which rows and columns should become zero.
→ **Crucial Snippet:**
```cpp
vector<int> row(n,0), col(m,0);

if(mat[i][j]==0){
    row[i]=1;
    col[j]=1;
}

if(row[i] || col[j])
    mat[i][j]=0;
```
→ **TC / SC:** `O(nm)` / `O(n+m)`

---

## OPTIMAL

→ **First instinct:** *"I immediately use the first row and first column as marker arrays to eliminate the extra space."*

→ **Core idea:** Use the **first column** to store row markers and the **first row** to store column markers. Maintain a separate variable `col0` because `mat[0][0]` cannot represent both the first row and first column. Mark in the first pass, then traverse from **bottom-right** to update the matrix without destroying the markers.

→ **Crucial Snippets:**
```cpp
int col0 = 1;
```

```cpp
if(mat[i][0]==0)
    col0=0;
```

```cpp
if(mat[i][j]==0){
    mat[i][0]=0;
    mat[0][j]=0;
}
```

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

→ **TC / SC:** `O(nm)` / `O(1)`

---

## WATCH OUT FOR

→ Writing `mat[0][1]=0` instead of `mat[0][j]=0`, or forgetting the separate `col0` variable for the first column.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Immediate modification causes cascading zeros, so I must mark first.
2. Brute force stores all zero positions.
3. Better solution stores row and column markers using two arrays.
4. Optimal solution reuses the first row and first column as marker arrays with `col0` for the first column.
5. Traverse from bottom-right to preserve markers and achieve `O(nm)` time with `O(1)` extra space.
````
