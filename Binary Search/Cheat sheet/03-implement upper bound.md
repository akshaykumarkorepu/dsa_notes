
# Implement Upper Bound

## PATTERN: Binary Search on Answer (First True)
→ **Trigger:** "when I see a sorted array and need the first element/index strictly greater than a target"

---

## BRUTE FORCE
→ **Idea:** Linearly scan the array and return the first index where `arr[i] > target`; if none exists, return `n`.
→ **TC / SC:** **O(n)** / **O(1)**

---

## OPTIMAL
→ **First instinct:** "I immediately binary search for the first index where `arr[mid] > target`."

→ **Core idea:** Maintain an answer `ans = n`. During binary search, if `arr[mid] > target`, store `mid` as a possible answer and continue searching the left half (`high = mid - 1`) to find an earlier valid index. Otherwise (`arr[mid] <= target`), move right (`low = mid + 1`) since the upper bound cannot be on the left.

**Crucial Snippets**

```cpp
int ans = n;
```

```cpp
if (arr[mid] > target) {
    ans = mid;
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

→ **TC / SC:** **O(log n)** / **O(1)**

---

## WATCH OUT FOR
→ Using `>=` instead of `>` (that's **Lower Bound**, not **Upper Bound**).

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Array is sorted, so Binary Search is applicable.
2. I need the **first index** where `arr[i] > target`.
3. Maintain `ans = n` as the default "not found" answer.
4. If `arr[mid] > target`, save it and search left; otherwise search right.
5. Return `ans`.
````
