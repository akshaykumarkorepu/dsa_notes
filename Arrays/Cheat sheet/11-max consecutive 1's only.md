
## Question: Maximum Consecutive Ones

### PATTERN: Linear Traversal + Running Count
→ **Trigger:** "when I see longest consecutive occurrence of one specific value (1s, 0s, vowels, even numbers, positives, etc.)"

---

### BRUTE FORCE

→ **Idea:** Start from every index containing `1`, count consecutive `1`s, and keep the maximum.

**Crucial Snippet**
```cpp
if(nums[i] == 1)
{
    int count = 0;
    for(int j = i; j < n && nums[j] == 1; j++)
        count++;

    maxCount = max(maxCount, count);
}
```

→ **TC / SC:** **O(N²)** / **O(1)**

---

### OPTIMAL

→ **First instinct:** "I immediately traverse once while maintaining the current streak of 1s."

→ **Core idea:** Maintain two variables:
- `currentCount` → current consecutive 1's.
- `maxCount` → maximum consecutive 1's seen so far.

Traverse the array once. If the current element is `1`, increment `currentCount`; otherwise, reset it to `0` because the streak is broken. Update `maxCount` after every iteration.

**Crucial Snippet**
```cpp
if(num == 1)
    currentCount++;
else
    currentCount = 0;

maxCount = max(maxCount, currentCount);
```

→ **TC / SC:** **O(N)** / **O(1)**

---

### WATCH OUT FOR

→ Initialize `currentCount` and `maxCount` to **0**, not **1**, because the array may contain no `1`s.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force counts consecutive 1's starting from every index, causing repeated work.
2. I can process the array in one pass by maintaining the current streak of 1's.
3. I'll keep `currentCount` for the ongoing streak and `maxCount` for the answer.
4. If I see a `1`, I increment the streak; otherwise, I reset it to `0`.
5. Update the maximum after every iteration and return it.
