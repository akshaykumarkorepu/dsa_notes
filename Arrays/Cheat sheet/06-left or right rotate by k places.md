
# Question: Rotate Array (Left Rotation)

### PATTERN: Array Rotation + Three-Reversal Pattern
→ **Trigger:** "When I see in-place array rotation, circular movement, O(1) extra space, or rotate left by d positions."

---

### BRUTE FORCE
→ **Idea:** Store first `d` elements → Shift remaining elements left using `arr[i-d] = arr[i]` → Copy stored elements to the last `d` positions using `arr[n-d+i] = temp[i]`.
→ **Formula:**
- Store: `temp = arr[0...d-1]`
- Shift: `arr[i-d] = arr[i]`
- Copy Back: `arr[n-d+i] = temp[i]`
→ **TC / SC:** `O(n)` / `O(d)`

---

### OPTIMAL
→ **First instinct:** "I immediately think of the Three-Reversal Algorithm."
→ **Core idea:** Normalize using `d %= n`. Reverse the first `d` elements, reverse the remaining `n-d` elements, then reverse the entire array to transform `[A | B] → [B | A]`.
→ **Formula:**
- `reverse(0, d-1)`
- `reverse(d, n-1)`
- `reverse(0, n-1)`
→ **TC / SC:** `O(n)` / `O(1)`

---

### WATCH OUT FOR
→ Forgetting `d %= n` before performing the rotation.

---

### INTERVIEW FLOW (what I say out loud, in order)
1. Normalize rotations using `d %= n`.
2. Brute force uses a temporary array and shifting.
3. Since in-place is required, I'll use the Three-Reversal Algorithm.
4. Reverse first `d`, reverse remaining, then reverse the whole array.
5. Time `O(n)`, Space `O(1)`.

---

# Question: Rotate Array (Right Rotation)

### PATTERN: Array Rotation + Three-Reversal Pattern
→ **Trigger:** "When I see in-place array rotation, circular movement, O(1) extra space, or rotate right by d positions."

---

### BRUTE FORCE
→ **Idea:** Store last `d` elements → Shift remaining elements right using `arr[i+d] = arr[i]` → Copy stored elements to the first `d` positions using `arr[i] = temp[i]`.
→ **Formula:**
- Store: `temp = arr[n-d...n-1]`
- Shift: `arr[i+d] = arr[i]`
- Copy Back: `arr[i] = temp[i]`
→ **TC / SC:** `O(n)` / `O(d)`

---

### OPTIMAL
→ **First instinct:** "I immediately think of the Three-Reversal Algorithm."
→ **Core idea:** Normalize using `d %= n`. Reverse the first `n-d` elements, reverse the last `d` elements, then reverse the entire array to transform `[A | B] → [B | A]`.
→ **Formula:**
- `reverse(0, n-d-1)`
- `reverse(n-d, n-1)`
- `reverse(0, n-1)`
→ **TC / SC:** `O(n)` / `O(1)`

---

### WATCH OUT FOR
→ Using the left-rotation reversal order instead of the right-rotation reversal order.

---

### INTERVIEW FLOW (what I say out loud, in order)
1. Normalize rotations using `d %= n`.
2. Brute force stores the last `d` elements and shifts right.
3. For the optimal solution, I'll use the Three-Reversal Algorithm.
4. Reverse first `n-d`, reverse last `d`, then reverse the whole array.
5. Time `O(n)`, Space `O(1)`.
```
