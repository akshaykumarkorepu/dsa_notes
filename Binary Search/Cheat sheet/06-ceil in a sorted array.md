
# Ceil in a Sorted Array

## PATTERN: First True Binary Search (Lower Bound)
→ **Trigger:** "when I see the first/leftmost element satisfying a condition in a sorted array (smallest element ≥ x, ceil, lower bound)"

---

## BRUTE FORCE

→ **Idea:** Traverse from left to right and return the first index where `arr[i] >= x`.

**Crucial Snippet**

```cpp
for(int i = 0; i < n; i++) {
    if(arr[i] >= x)
        return i;
}
return -1;
```

→ **TC / SC:** `O(N)` / `O(1)`

---

## OPTIMAL

→ **First instinct:** "I immediately binary search for the first element `>= x`."

→ **Core idea:** Maintain `ans = -1`. If `arr[mid] >= x`, store `mid` as a potential ceil and continue searching **left** (`high = mid - 1`) because there may be a smaller valid element. Otherwise, move **right** (`low = mid + 1`). The final `ans` is the first occurrence of the ceil.

**Crucial Snippets**

```cpp
int ans = -1;
```

```cpp
if(arr[mid] >= x) {
    ans = mid;
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

→ **TC / SC:** `O(log N)` / `O(1)`

---

## WATCH OUT FOR

→ Moving **right** after finding `arr[mid] >= x`; always move **left** to ensure you get the **first occurrence**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so Binary Search is applicable.
2. I need the **first** element satisfying `arr[i] >= x`.
3. Whenever `arr[mid] >= x`, I store it as a candidate answer and move left.
4. If `arr[mid] < x`, I discard the left half and move right.
5. The stored answer is the ceil index; if none exists, return `-1`.
````
