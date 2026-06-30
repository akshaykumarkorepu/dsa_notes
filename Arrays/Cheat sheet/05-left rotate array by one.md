

## Question: Rotate Array by One (Clockwise)

### PATTERN: Array Element Shifting (In-place Rotation)
→ **Trigger:** "When I see a one-position rotation or shifting elements while preserving order."

---

### BRUTE FORCE
**Not necessary.** The in-place solution is already the simplest and optimal approach. Using an extra array only increases space complexity without simplifying the logic.

---

### OPTIMAL

→ **First instinct:** "I immediately save the last element, shift everything one position to the right, then place the saved element at the front."

→ **Core idea:** Store the last element in a variable `last`. Traverse the array from **right to left**, updating `arr[i] = arr[i-1]` so values aren't overwritten. Finally, assign `arr[0] = last` to complete the rotation.

→ **TC / SC:** **O(n)** / **O(1)**

---

### WATCH OUT FOR

→ **Don't shift from left to right**—it overwrites elements before they've been copied. For a right shift, always traverse **right to left**.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Save the last element before it gets overwritten.
2. Traverse from right to left.
3. Shift each element one position to the right.
4. Place the saved last element at index `0`.
5. Rotation completed in-place with **O(n)** time and **O(1)** space.
```
