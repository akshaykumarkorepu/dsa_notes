
## PATTERN: Linear Traversal (Single Pass)

→ **Trigger:** "when I see find the largest/smallest/maximum/minimum/sum/count in an array"

---

## BRUTE FORCE

→ **Idea:** Sort the array and return the last element.

→ **TC / SC:** `O(n log n)` / `O(1)` *(or `O(log n)` recursion stack depending on sort implementation)*

---

## OPTIMAL

→ **First instinct:** "I immediately keep a running maximum while scanning the array once."

→ **Core idea:** Initialize `maxi = arr[0]`. Traverse from index `1`; whenever `arr[i] > maxi`, update `maxi`. After one complete pass, `maxi` stores the largest element.

→ **TC / SC:** `O(n)` / `O(1)`

---

## WATCH OUT FOR

→ Initializing `maxi = 0` instead of `arr[0]` (fails for arrays containing negative numbers in the general case).

---

## INTERVIEW FLOW

1. Brute force: sort the array and return the last element (`O(n log n)`).
2. Better approach: sorting is unnecessary since only the maximum is needed.
3. Maintain a running maximum initialized with the first element.
4. Scan once and update `maxi` whenever a larger element is found.
5. Return the final maximum (`O(n)` time, `O(1)` space).
```
