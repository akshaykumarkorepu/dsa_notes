## PATTERN: Maintain Multiple Extremes (Tracking Maximums)
→ **Trigger:** "when I see find the second largest/smallest, kth extreme (small K), or multiple maximum/minimum values in one traversal"

---

## BRUTE FORCE
→ **Idea:** Sort the array, then traverse backwards to find the first element different from the largest.
→ **TC / SC:** `O(N log N)` / `O(1)`

---

## OPTIMAL
→ **First instinct:** "I immediately maintain two variables: `largest` and `secondLargest` while traversing once."

→ **Core idea:** Traverse the array once while maintaining `largest` and `secondLargest`. If the current element is greater than `largest`, shift the old `largest` to `secondLargest` and update `largest`. Otherwise, if `current < largest` and `current > secondLargest`, update `secondLargest`. The `current < largest` condition prevents duplicate maximum values from becoming the second largest.

→ **TC / SC:** `O(N)` / `O(1)`

---

## WATCH OUT FOR
→ Forgetting the condition `current < largest`, which causes duplicate largest elements to be incorrectly considered as the second largest.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Sorting works but takes `O(N log N)`.
2. I'll optimize using a single traversal.
3. Maintain `largest` and `secondLargest`.
4. Update both when a new maximum is found; otherwise update only `secondLargest` if it's between them.
5. Return `secondLargest` (or `-1` if it was never updated).
