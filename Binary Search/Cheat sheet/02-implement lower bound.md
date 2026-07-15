

**Question:** Implement Lower Bound

---

## PATTERN: Binary Search (First True / Lower Bound)
→ **Trigger:** "when I see a sorted array and need the first index where `arr[i] >= target` (lower bound / search insert position)"

---

## BRUTE FORCE
→ **Idea:** Linearly scan the array and return the first index where `arr[i] >= target`; if none exists, return `n`.
→ **TC / SC:** **O(N)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately Binary Search for the first index satisfying `arr[mid] >= target`."

→ **Core idea:** Maintain `low`, `high`, and `ans = n`. If `arr[mid] >= target`, store `mid` as a possible answer (`ans = mid`) and search the left half (`high = mid - 1`) to find an earlier valid index. Otherwise, move right (`low = mid + 1`) since the current value and everything before it are too small.

**Crucial Snippet:**
```cpp
int ans = n;

if (arr[mid] >= target) {
    ans = mid;
    high = mid - 1;
} else {
    low = mid + 1;
}
```

→ **TC / SC:** **O(log N)** / **O(1)**

---

## WATCH OUT FOR

→ Don't return immediately when `arr[mid] >= target`; **save the answer and continue searching left**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so I'll use Binary Search.
2. I'm searching for the **first index** where `arr[i] >= target`, not necessarily the target itself.
3. If `arr[mid] >= target`, I store `mid` and search left.
4. Otherwise, I search the right half.
5. If no valid index is found, `ans` remains `n`, which is the required answer.
````
