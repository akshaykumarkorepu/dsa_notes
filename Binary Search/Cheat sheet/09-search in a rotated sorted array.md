
# Search in Rotated Sorted Array

**PATTERN:** Modified Binary Search (Rotated Sorted Array)
→ **Trigger:** "when I see a rotated sorted array with distinct elements and need to search in O(log n)"

---

## BRUTE FORCE

→ **Idea:** Linearly traverse the array and return the index if the target is found.

→ **TC / SC:** **O(n)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately identify which half is sorted, then check whether the target lies inside that sorted half."

→ **Core idea:**
Maintain `low`, `high`, and `mid` exactly like Binary Search. In every iteration, first check if `arr[mid]` is the target. Then determine the sorted half using `arr[low] <= arr[mid]`. If the left half is sorted, check whether the target lies within `[arr[low], arr[mid])`; otherwise search the right half. If the left half isn't sorted, the right half must be sorted, so check whether the target lies within `(arr[mid], arr[high]]`; otherwise search the left half.

**Crucial Snippets**

```cpp
if(arr[mid] == key)
    return mid;
```

```cpp
if(arr[low] <= arr[mid]) {
    // Left half sorted
}
else {
    // Right half sorted
}
```

```cpp
if(arr[low] <= key && key < arr[mid])
    high = mid - 1;
else
    low = mid + 1;
```

```cpp
if(arr[mid] < key && key <= arr[high])
    low = mid + 1;
else
    high = mid - 1;
```

→ **TC / SC:** **O(log n)** / **O(1)**

---

## WATCH OUT FOR

→ Forgetting to check `arr[mid] == key` **before** deciding the sorted half, or using incorrect comparison operators (`<` vs `<=`) in the range checks.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Check if `mid` itself is the target.
2. Identify which half is sorted using `arr[low] <= arr[mid]`.
3. Check whether the target lies inside the sorted half.
4. Discard the impossible half and continue Binary Search.
5. Repeat until found or `low > high`.
````
