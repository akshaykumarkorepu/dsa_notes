
# Floor in a Sorted Array

## PATTERN: Last True Binary Search (Upper Bound - 1)
→ **Trigger:** "when I see the last/rightmost element satisfying a condition in a sorted array (largest element ≤ x, floor, upper bound - 1)"

---

## BRUTE FORCE
→ **Idea:** Traverse the array and keep updating the answer whenever `arr[i] <= x`; the last updated index is the floor.
→ **TC / SC:** `O(N)` / `O(1)`

---

## OPTIMAL

→ **First instinct:** "I immediately binary search for the last element `<= x`."

→ **Core idea:** Maintain `ans = -1`. During Binary Search, if `arr[mid] <= x`, store `mid` in `ans` (current valid floor) and search **right** (`low = mid + 1`) for a larger valid element. Otherwise, search **left** (`high = mid - 1`). The final `ans` is the floor index (last occurrence automatically handled).

**Crucial Snippets**

```cpp
int ans = -1;
```

```cpp
if (arr[mid] <= x) {
    ans = mid;
    low = mid + 1;
}
else {
    high = mid - 1;
}
```

→ **TC / SC:** `O(log N)` / `O(1)`

---

## WATCH OUT FOR

→ Returning immediately when `arr[mid] == x`; continue searching **right** to get the **last occurrence**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so I'll use Binary Search.
2. I need the **last** element satisfying `arr[i] <= x`.
3. Whenever `arr[mid] <= x`, I store it as a candidate answer and move right.
4. If `arr[mid] > x`, I discard the right half and move left.
5. The stored answer is the floor index; if none exists, return `-1`.
````
