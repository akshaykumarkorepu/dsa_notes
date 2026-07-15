
## PATTERN: Kadane's Algorithm Variant (Track Maximum & Minimum Product)
→ **Trigger:** "when I see a contiguous subarray with **product**, especially involving negative numbers and zeros"

---

## BRUTE FORCE
→ **Idea:** Generate every subarray, maintain a running product while extending it, and update the maximum product found.
→ **TC / SC:** O(N²) / O(1)

---

## OPTIMAL

→ **First instinct:** "I immediately track both the maximum and minimum product ending at the current index because a negative can flip them."

→ **Core idea:** Maintain `maxProd` and `minProd`, representing the maximum and minimum product of a subarray ending at the current index. If the current element is negative, swap them first since signs flip. Then update both by either starting a new subarray (`arr[i]`) or extending the previous one (`arr[i] * maxProd` / `arr[i] * minProd`). Update the global answer using `maxProd`.

**Crucial Snippets**

```cpp
if(arr[i] < 0)
    swap(maxProd, minProd);
```

```cpp
maxProd = max(arr[i], arr[i] * maxProd);
```

```cpp
minProd = min(arr[i], arr[i] * minProd);
```

```cpp
ans = max(ans, maxProd);
```

→ **TC / SC:** O(N) / O(1)

---

## WATCH OUT FOR

→ Forgetting to **swap `maxProd` and `minProd` when the current element is negative**, which breaks all negative-number cases.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every subarray in O(N²).
2. Product behaves differently from sum because negatives flip signs.
3. Track both maximum and minimum product ending at each index.
4. Swap them when the current element is negative, then update both states.
5. Update the global answer using the current maximum product.
````
