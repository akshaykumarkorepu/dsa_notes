
# Question: Check if Array is Sorted

**PATTERN:** Linear Scan (Adjacent Comparison)  
→ **Trigger:** "when I see a question asking to verify whether an array maintains increasing/non-decreasing order"

---

## BRUTE FORCE

**Not necessary.**  
The optimal linear scan is the only sensible approach expected in interviews.

---

## OPTIMAL

→ **First instinct:** "I immediately scan the array once and compare every element with its next element."

→ **Core idea:** Maintain only the current index `i`. Traverse from `0` to `n-2`; if `arr[i] > arr[i+1]`, return `false` immediately since the sorted property is violated. If the traversal completes without any violation, return `true`.

→ **TC / SC:** **O(n)** / **O(1)**

---

## WATCH OUT FOR

→ Loop only until `n-2` (`i < n-1`); otherwise accessing `arr[i+1]` causes an out-of-bounds error.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. A sorted array means every adjacent pair satisfies `arr[i] <= arr[i+1]`.
2. I'll perform one linear traversal.
3. If I find `arr[i] > arr[i+1]`, I'll immediately return `false`.
4. Otherwise, I'll continue checking all adjacent pairs.
5. If no violation is found, I'll return `true` in **O(n)** time and **O(1)** space.
```
