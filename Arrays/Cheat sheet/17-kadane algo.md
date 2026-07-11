

## Question: Maximum Subarray Sum (Kadane's Algorithm)

### PATTERN: Running Sum (Kadane's Algorithm)
→ **Trigger:** *"when I see maximum/minimum sum of a contiguous subarray."*

---

### BRUTE FORCE
→ **Idea:** Generate every subarray, maintain a running sum for each starting index, and update the maximum sum.
→ **Key Snippet:**
```cpp
sum += arr[j];
maxSum = max(maxSum, sum);
```
→ **TC / SC:** `O(n²) / O(1)`

---

### OPTIMAL
→ **First instinct:** *"I immediately think of Kadane's Algorithm because a negative running sum can never help a future subarray."*

→ **Core idea:** Maintain a running sum `sum` and the best answer `maxSum`. For every element, extend the current subarray (`sum += arr[i]`), update the answer, and if `sum` becomes negative, reset it to `0` since carrying a negative prefix only reduces future sums.

→ **Key Snippets:**
```cpp
sum += arr[i];
```

```cpp
maxSum = max(maxSum, sum);
```

```cpp
if(sum < 0)
    sum = 0;
```

→ **TC / SC:** `O(n) / O(1)`

---

### WATCH OUT FOR
→ Initialize `maxSum = INT_MIN` (not `0`), otherwise all-negative arrays fail.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force generates every subarray and computes its sum.
2. Observe that a negative running sum can never improve a future subarray.
3. Maintain a running sum and extend the current subarray.
4. Update the maximum before resetting the running sum.
5. If the running sum becomes negative, discard it and continue.

---

# DSA SHORT NOTES

## Question: Print Maximum Sum Subarray

### PATTERN: Kadane's Algorithm + Index Tracking
→ **Trigger:** *"when I see return/print the maximum sum subarray or its indices."*

---

### BRUTE FORCE
→ **Idea:** Generate every subarray and whenever a better sum is found, store its start and end indices.
→ **Key Snippet:**
```cpp
if(sum > maxSum){
    start = i;
    end = j;
}
```
→ **TC / SC:** `O(n²) / O(1)`

---

### OPTIMAL
→ **First instinct:** *"I immediately use Kadane and additionally track where the current candidate subarray starts."*

→ **Core idea:** Maintain `sum`, `maxSum`, `tempStart`, `start`, and `end`. `tempStart` stores the beginning of the current candidate. Whenever the current sum becomes the best, save `start = tempStart` and `end = i`. If `sum` becomes negative, reset `sum` and move `tempStart` to `i + 1`.

→ **Key Snippets:**
```cpp
if(sum > maxSum){
    start = tempStart;
    end = i;
}
```

```cpp
if(sum < 0){
    sum = 0;
    tempStart = i + 1;
}
```

```cpp
for(int i = start; i <= end; i++)
    ans.push_back(arr[i]);
```

→ **TC / SC:** `O(n) / O(1)`

---

### WATCH OUT FOR
→ Update `start = tempStart`, **not** `start = i`; otherwise the printed subarray will be incorrect.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force stores indices for every better subarray.
2. Apply Kadane to avoid checking every starting index.
3. Track the current candidate using `tempStart`.
4. Save `start` and `end` only when a new maximum is found.
5. Reset `sum` and move `tempStart` when the running sum becomes negative.
````
