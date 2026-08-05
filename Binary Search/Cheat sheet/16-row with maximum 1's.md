

## Question: Row with Maximum 1s (Row-wise Sorted Matrix)

### PATTERN: Binary Search on Every Row (First Occurrence)
→ **Trigger:** "When I see a row-wise sorted binary matrix and need to count/find the maximum number of 1s."

---

### BRUTE FORCE
→ **Idea:** Traverse every element, count the number of 1s in each row, and keep track of the row with the maximum count.

**Key Snippet**

```cpp
int count = 0;

for(int j = 0; j < m; j++){
    if(arr[i][j] == 1)
        count++;
}

if(count > maxOnes){
    maxOnes = count;
    ans = i;
}
```

→ **TC / SC:** `O(N × M)` / `O(1)`

---

### OPTIMAL

→ **First instinct:** "I immediately Binary Search every row to find the first occurrence of 1."

→ **Core idea:** Since every row is sorted, Binary Search each row to find the leftmost `1` (`firstOne`). Compute the number of ones as `m - firstOne` (or `0` if no `1` exists). Maintain `maxOnes` and `ans`, updating them only when the current row has strictly more ones.

**Key Snippets**

**Find First 1**

```cpp
int firstOne = -1;

while(low <= high){

    int mid = low + (high - low)/2;

    if(arr[i][mid] == 1){
        firstOne = mid;
        high = mid - 1;
    }
    else{
        low = mid + 1;
    }
}
```

**Count Ones**

```cpp
int ones = (firstOne == -1) ? 0 : m - firstOne;
```

**Update Answer**

```cpp
if(ones > maxOnes){
    maxOnes = ones;
    ans = i;
}
```

→ **TC / SC:** `O(N log M)` / `O(1)`

---

### WATCH OUT FOR

→ Using `>=` instead of `>` while updating the answer. The problem asks for the **first** row with the maximum number of 1s.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Every row is sorted, so I don't need to count every element.
2. I'll Binary Search each row to find the first occurrence of `1`.
3. Number of ones is `m - firstOneIndex`.
4. Maintain `maxOnes` and the corresponding row index.
5. Update only when `ones > maxOnes` to preserve the first row in case of ties.
````
