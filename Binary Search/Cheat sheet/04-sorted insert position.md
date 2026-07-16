
# Sorted Insert Position

## PATTERN: Lower Bound Binary Search
→ **Trigger:** "when I see a sorted array asking for the insertion position, first element ≥ target, or maintain sorted order"

---

## BRUTE FORCE
→ **Idea:** Linearly scan the array and return the first index where `arr[i] >= k`; if none exists, return `n`.
→ **TC / SC:** **O(N)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately think of Lower Bound Binary Search."

→ **Core idea:** Maintain `low`, `high`, and `ans = n`. Whenever `arr[mid] >= k`, store `mid` as a possible answer (`ans = mid`) and search the left half (`high = mid - 1`) for an earlier valid index. Otherwise, search the right half (`low = mid + 1`). The final `ans` is the first index where `arr[i] >= k`, which is either the target's index or its insertion position.

**Crucial Snippet:**
```cpp
int ans = n;

if(arr[mid] >= k){
    ans = mid;
    high = mid - 1;
}
else{
    low = mid + 1;
}
```

→ **TC / SC:** **O(log N)** / **O(1)**

---

## WATCH OUT FOR

→ Using `arr[mid] > k` instead of `arr[mid] >= k`; that computes the **Upper Bound** and gives the wrong answer when `k` already exists.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so I'll use Binary Search.
2. This is a Lower Bound problem—find the first index where `arr[i] >= k`.
3. Maintain `ans = n` as the current best insertion position.
4. On `arr[mid] >= k`, update `ans` and move left; otherwise move right.
5. Return `ans`, which is either the target's index or the correct insertion position.
```
````
