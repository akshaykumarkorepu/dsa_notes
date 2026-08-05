

## Question: Row with Maximum 1s (Row-wise Sorted Matrix)

### PATTERN: Binary Search on Every Row (First Occurrence)
→ **Trigger:** "When I see a row-wise sorted binary matrix and need to count/find the maximum number of 1s."

---

### BRUTE FORCE
→ **Idea:** Traverse every element, count the number of 1s in each row, and keep track of the row with the maximum count.

**Cheat Code Snippet**

```cpp
for(each row){

    count = 0;

    for(each col)
        if(arr[i][j] == 1)
            count++;

    if(count > maxOnes){
        maxOnes = count;
        ans = i;
    }
}
```

**Remember**

```
Count every 1
      ↓
Keep maximum count
```

→ **TC / SC:** `O(N × M)` / `O(1)`

---

### OPTIMAL

→ **First instinct:** "I immediately Binary Search every row to find the first occurrence of 1."

→ **Core idea:** Since every row is sorted, Binary Search each row to find the leftmost `1`. Compute the number of ones as `m - firstOne`. Maintain `maxOnes` and `ans`, updating them only if the current row has more ones.

### Cheat Code Snippet 1 — Binary Search

```cpp
while(low <= high){

    if(arr[i][mid] == 1){
        firstOne = mid;
        high = mid - 1;   // go left
    }
    else{
        low = mid + 1;    // go right
    }
}
```

**Memory**

```
Found 1
 ↓
Store Answer
 ↓
Move LEFT
```

---

### Cheat Code Snippet 2 — Count Ones

```cpp
ones = (firstOne == -1) ? 0 : m - firstOne;
```

**Memory**

```
Ones = Columns - First 1 Position
```

---

### Cheat Code Snippet 3 — Update Answer

```cpp
if(ones > maxOnes){

    maxOnes = ones;
    ans = i;
}
```

**Memory**

```
STRICTLY GREATER

>

Never >=
```

---

### Entire Flow (10-second Recall)

```text
For every row

        ↓

Binary Search First 1

        ↓

ones = m - firstOne

        ↓

if(ones > maxOnes)

        ↓

Update Answer
```

→ **TC / SC:** `O(N log M)` / `O(1)`

---

### WATCH OUT FOR

→ Using `>=` instead of `>`. This changes the answer from the **first** row with maximum 1s to the **last** row.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Every row is sorted, so counting isn't necessary.
2. I'll Binary Search each row to find the first `1`.
3. Ones in that row = `m - firstOneIndex`.
4. Maintain `maxOnes` and `ans`.
5. Update only when `ones > maxOnes` to keep the first row.
````
