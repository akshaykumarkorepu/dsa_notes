
# Alternate Positive Negative

**PATTERN:** Stable Partition + Merge (Two Auxiliary Arrays + Three Pointers)  
→ **Trigger:** "when I see *maintain relative order while rearranging/alternating two groups*"

---

## BRUTE FORCE / OPTIMAL (Same for this question)

> **Note:** Brute Force and Optimal are the **same** because maintaining the relative order (stable ordering) makes the auxiliary-array approach the intended and optimal solution.

→ **First instinct:** "I immediately separate positives and negatives into two vectors, then merge them alternately."

→ **Core idea:**
- Traverse the array once and store all positives (including `0`) in `pos` and negatives in `neg`.
- Maintain three pointers: `p` (positive vector), `n` (negative vector), and `i` (original array).
- Fill the original array alternately using `pos` and `neg`. When one vector is exhausted, append the remaining elements from the other vector.

**Crucial snippets to remember:**

```cpp
vector<int> pos;
vector<int> neg;
```

```cpp
for (int x : arr) {

    if (x >= 0)
        pos.push_back(x);
    else
        neg.push_back(x);
}
```

```cpp
int p = 0;
int n = 0;
int i = 0;
```

```cpp
while (p < pos.size() && n < neg.size()) {

    arr[i] = pos[p];
    i++;
    p++;

    arr[i] = neg[n];
    i++;
    n++;
}
```

```cpp
while (p < pos.size()) {

    arr[i] = pos[p];
    i++;
    p++;
}
```

```cpp
while (n < neg.size()) {

    arr[i] = neg[n];
    i++;
    n++;
}
```

→ **TC / SC:**

- **Time:** `O(N)`
- **Space:** `O(N)`

---

## WATCH OUT FOR

→ Treat **0 as a positive number** (`x >= 0`), not negative.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Relative order must be preserved, so swapping isn't suitable.
2. Separate positives and negatives into two vectors.
3. Use three pointers (`p`, `n`, `i`) to merge them alternately.
4. Append whichever vector has remaining elements.
5. Time `O(N)`, Space `O(N)` because the auxiliary vectors together store `N` elements.
````
