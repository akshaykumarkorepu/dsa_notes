
# First and Last Occurrence in Sorted Array

## PATTERN: Boundary Binary Search (Modified Binary Search)
→ **Trigger:** "when I see a sorted array and need the first/last occurrence, leftmost/rightmost index, or range of an element"

---

## BRUTE FORCE
→ **Idea:** Traverse the array once, store the first occurrence only once, and keep updating the last occurrence whenever `x` is found.

**Crucial Snippet**
```cpp
if (arr[i] == x) {
    if (first == -1) first = i;
    last = i;
}
```

→ **TC / SC:** `O(n)` / `O(1)`

---

## OPTIMAL

→ **First instinct:** "I immediately run Boundary Binary Search twice—once for the left boundary and once for the right boundary."

→ **Core idea:** Maintain `low`, `high`, `mid`, and `ans`. Whenever `arr[mid] == x`, store `ans = mid` instead of stopping. For the **first occurrence**, continue searching left using `high = mid - 1`. For the **last occurrence**, continue searching right using `low = mid + 1`. Finally return both indices.

**Crucial Snippets**

**First Occurrence**
```cpp
if (arr[mid] == x) {
    ans = mid;
    high = mid - 1;
}
```

**Last Occurrence**
```cpp
if (arr[mid] == x) {
    ans = mid;
    low = mid + 1;
}
```

**Return**
```cpp
return {firstOccurrence(arr, x), lastOccurrence(arr, x)};
```

→ **TC / SC:** `O(log n)` / `O(1)`

---

## WATCH OUT FOR

→ Stopping immediately after finding `x`. Always **store the answer first**, then continue searching toward the required boundary.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Since the array is sorted, I'll use Binary Search.
2. I need boundaries, not just any occurrence.
3. I'll find the first occurrence by moving left after finding the target.
4. I'll find the last occurrence by moving right after finding the target.
5. Return `{firstOccurrence, lastOccurrence}`.
````
